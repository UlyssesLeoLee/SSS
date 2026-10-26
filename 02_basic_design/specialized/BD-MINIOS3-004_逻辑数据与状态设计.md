# BD-MINIOS3-004 — 逻辑数据、版本模型与状态机基本设计

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 对应：FR-CORE, FR-GIT-004/006/009/010, FR-S3-005/011/012/019, FR-XCHG-008/009/010, FR-OPS-012。

## 1. 命名与概念ER

模型源码：`03_diagrams/04_domain_er.mmd`。本章给出逻辑实体属性和约束，不承诺已设计物理SQL表。键由Rust强类型管理，不允许裸String随意交叉使用；所有租户关联显式tenant_id。

| 实体/聚合 | 主键（逻辑） | 主要属性 | 一致性/生命周期 |
|---|---|---|---|
| Tenant | tenant_id | name, residency_policy, status, quota | Global Raft |
| Project | project_id | tenant_id, role mappings | Global Raft |
| Principal/Policy | principal_id/policy_id | policy_revision, deny/allow, condition | Global Raft；撤权更改epoch |
| StorageBackend | backend_id | vendor, endpointRef, secretRef, zone, cap_digest | Global Raft；能力需证据 |
| StorageBucket | bucket_id | tenant_id, external_name, versioning_state, lock_config, placement_policy, routing_epoch | Global Raft |
| ObjectHead | tenant_id+bucket_id+key | current_logical_version, sequence, delete_marker | 按Key范围Raft组 |
| ObjectVersion | bucket_id+key+logical_version_id | created_at, length, etag, checksum, retention, hold, state | 线性化发布，不可改写原字节 |
| PhysicalPlacement | object_version+replica_id | backend_id, physical_bucket/key/versionId, manifest_digest, epoch, verified_at | 单逻辑版本可多物理副本 |
| CASContent | scope_id+sha256_oid+size | manifest, state, refs, retained_until | 独立不可变内容；默认tenant级去重 |
| LfsRepoGrant | repo_id+oid | tenant_id, authorization, link_revision | 授权与CAS existence分离 |
| GitCommitIndex | repo_id+commit_sha+path | oid, size, index_watermark, source_event | 派生，只读镜像Git真相 |
| GitRefSnapshot | repo_id+snapshot_id | refs_digest, verified_at, watermark | GC安全检查依据 |
| MediaAsset | asset_id | type, dimensions, license, metadata | 派生；重建能力 |
| MultipartUpload | upload_id | bucket/key/tenant, parts, expire_at, state | 未发布部分可回收 |
| MigrationJob | job_id | source, target, phase, cursor, last_delta, approval | replicated state machine |
| AuditOutbox | event_id | actor, action, subject, timestamp, hash_prev, status | 与对象元数据事务内产生 |

## 2. 版本标识绝不可混用

`GitCommitSha`是Git仓库自己决定的提交标识；`LfsOidSha256`代表媒体内容hash；`LogicalS3VersionId`对外可寻址版本；`PhysicalVersionId`代表上游provider物理版本（可能不存在/变化）；Native则由generation+EC manifest唯一定位。Git commit→路径→LFS兼容OID可能关联多个S3逻辑版本或物理副本；任何“直接把commit hash设为versionId”的设计属于严重错误。

### 2.1 Versioning启停规则

- Enabled：每次成功PUT生成新的非null逻辑版本；不指定Version的DELETE生成新DeleteMarker；带指定Version的删除只处理目标版本且受WORM保护。
- Suspended：写入null版本语义需参照AWS规范逐场景模拟；不能把null误当普通永久版本。详细设计列出Enabled↔Suspended/现存版本与DeleteMarker交叉矩阵。
- 启用版本前历史未版本化对象：导入/切换时明确null语义和可恢复边界。
- GET/HEAD/LIST V2 在versioning启用时按当前可见版本返回；显式versionId查询取得历史对象或已记录的DeleteMarker/404语义。

### 2.2 外部版本迁移映射

```text
Public: tenant + bucket + key + LogicalVersionId = V100
                  ├── SourcePlacement: AWS S3 v=3HL...
                  ├── TargetPlacement: MinIO v=ab9...
                  └── NativePlacement: generation=G423, manifest=M56
```

外部VersionId始终为`V100`；内部VersionId由Map不同目的端驱动；切换后原路径继续有效。逻辑VersionId的序关系由元数据log index管理，不由时间字符串/供应商ID排序。

## 3. 关键唯一性、索引与约束

- namespace唯一 `(tenant_id, bucket_name)`；跨租户相同外部bucket name是否允许由Endpoint路由策略决定（S3标准桶命名兼容要评审）。默认**单一S3 Endpoint下bucket名全局唯一**，避免标准SDK路由歧义；内部bucket_id永不重用。
- `ObjectHead` Key按原始UTF-8字节进行S3规定的比较和列表排序，无路径正规化；对象Key可能包含`/`但无目录层级语义。
- `ObjectVersion` unique `(bucket_id,key,logical_version_id)`；latest pointer更新与version row插入在同一Raft状态机事务。
- `PhysicalPlacement` unique `(logical_version_id, backend_id, placement_generation)`；目标回写要核验manifest与physical VersionId。
- CAS内容唯一 `(tenant_scope,oid_sha256,size)`，同时维持Repository授权链接；授权校验不可只依CAS existence返回true。
- Outbox唯一`event_id`，消费外部sink至少一次，重试不可改变业务写入次数。
- Bucket路由切换使用全局`routing_epoch` CAS，读取路径必须携带版本/epoch后查询正确placement。

## 4. 元数据分区和强LIST

全局组处理Tenant/Policy/Bucket路由，Key范围组管理ObjectHead+Version的有序索引；E0 单范围组也运行一致接口。E1按 `(bucket_id,key_byte_range)` 划分ranges，每个range有自己的Raft group且有序KV迭代。ListObjectsV2的prefix过滤经过各range一致性barrier，按排序做有界merge与分页。**一次跨多个Raft组的快照并非天然全局原子快照**，BD不能据此承诺分页期间对并发写入“零重复零遗漏”；需要在DD定义多组统一snapshot fence或版本化分页视图、token语义及保证范围，测试高并发PUT/LIST及range迁移。若不能形式化，则E1只允许Bucket归一Raft group以获得强快照语义，且按性能测试决定扩展能力。不得虚报已解决分布式全局LIST。

## 5. 不可逆业务不变量（Inv）

| 编号 | 必须始终成立的条件 | 违约处理 |
|---|---|---|
| INV-01 | 已ACK S3版本的完整manifest可由足够数量经校验的分片或符合能力约束的provider读取 | 立即Sev1，不伪报404 |
| INV-02 | 同一LFS兼容OID只能对应相同字节+size；OID不能变更其内容 | 拒绝、冻结并审计 |
| INV-03 | 所有受Git活跃/保留引用的CAS不允许物理删除 | GC fail-closed |
| INV-04 | 特定Version的WORM retention不因路由/迁移而缩短 | 迁移Blocked |
| INV-05 | `ObjectHead.latest`必须指向同Key已发布版本或合法DeleteMarker | 抑制发布并修复 |
| INV-06 | 任何Bucket同时只有一个主写的routing_epoch | 旧epoch拒绝新写 |
| INV-07 | 已撤销权限的cache不能授予新的对象读取 | token/epoch验证失败拒绝 |
| INV-08 | AuditOutbox先于或同一目录事务形成、可重放 | 导出滞后触发报警/阻断高风险行为 |
| INV-09 | 迁移切换后Public LogicalVersionId集合不发生丢失或错序 | 回滚或停止切换 |
| INV-10 | 对象长度与签名校验在完成ACK之前有可靠证据 | 不发布成功 |

## 6. 状态机说明

`Object`: `Allocated→Receiving→PhysicallyDurable→Committed→(Superseded|DeleteMarked)→RetentionEligible→Tombstoned→Purged`；错误时允许`Abandoned`从未Committed的分支进入（孤儿回收）。`Committed`是唯一可对外可见的状态。`CAS`: `Quarantined/Verified/Linked/Unreferenced/PendingPurge/Deleted`；不能由`Unreferenced`直接物理删除。`Migration`: 详见BD-007；`LFS upload`: `Issued/Receiving/ChecksumVerified/DurabilityVerified/Published/Expired`。

## 7. 数据保存与备份边界

权威Raft snapshot+WAL必须保留元数据状态、membership、路由历史、AuditOutbox偏移与KMS密钥引用；数据分片备份/复制必须与snapshot形成一致性证据（snapshot具体索引），否则仅恢复目录会出现悬挂对象。异地存储中导出开放版本映射Manifest，可由第三方工具恢复Bucket/Key/LogicalVersion/PhysicalVersion、校验、metadata/tag以及保留策略。导出是否能保留法定WORM效力由目的端能力判定。

## 8. 计量与限额

Quota分为逻辑字节（含可计版本）、物理容量估算、计量请求、临时Multipart配额、并发流、迁移预算；预留/提交/回退必须在权威存储事务执行。逻辑计量不能对跨租户CAS去重泄漏真实物理占用；回收前实际盘位尚在需有`PendingReclaim`显示。

## 9. 完整性与审计证据模型

每个对象写入保存`checksum_type,checksum_value,upload_size,manifest_digest,placement_receipts,committed_raft_index,writer_auth_context_digest`; 每次数据自愈/迁移附`source_digest,target_digest,block_validation,tool_version,operation_id`；审计事件包含不可逆来源关联，但不写AccessKey明文、SSE-KMS DEK、media metadata敏感文本。

## 10. 详细设计待决事项

跨range原子List快照、compaction时保留历史Version、memtable/write amplification、Bucket名全局唯一与多租户虚拟地址兼容、超大Git refs snapshot计算、E1 metadata re-sharding回滚。列为P0，无形式化/故障验证不得对外宣称满足NFR-PER-003同时满足NFR-AVA-004。
