# BD-MINIOS3-007 — S3互换、迁移、复制、故障切换与灾备

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 对应 FR-XCHG-001~015、FR-OPS-012、NFR-MIG-001~004、NFR-AVA-001~006；风险P0。

## 1. 可替换存储契约

存储后端Adapter必须提供：名称/地址/region、凭据引用、可用存储API子集、Versioning/Lock/LegalHold/SSE/Checksum/Multipart/Tagging/Copy/DeleteMarker等能力、有效一致性承诺、数据驻留范围、max object、网络出口成本估计及已验证证据。**某后端接口存在 != 已具备合规能力**；接口支持状态按 `Supported/Conditional/Unsupported/Unverified` 精确定义。

## 2. 迁移作业状态机

主图：`../06_diagrams/05_migration_state.mmd`。

`Draft → Inventoried → CapabilityChecked → DryRunReviewed → Approved → InitialCopy → DeltaCatchup → Verify → WriteFence → FinalDelta → RouteCutover → Observe → Completed`。失败可到`Paused/Blocked/Retry/Abort/Compensating/Rollback`，不支持从`Blocked`直接强制`RouteCutover`。撤销作业不允许自动删源。每阶段记录`job_id,tenant,bucket,source/target,route_epoch,source_watermark,copy_checkpoint,version_count,bytes,checksum_report,conflicts,approval_id,rollback_window`。

## 3. 枚举与一致性水位

源扫描包含全部版本、delete marker、tag/metadata、Retention/LegalHold、ACL/policy、KMS要求；源本身不能枚举历史版本时**标阻断**而不是仅迁当前版本。大对象采用streaming/Multipart transfer，不把全部文件加载内存。首次inventory后记录watermark，持续捕获所有经MinIOS3网关的写入变更；**若上游允许绕过本网关直接写，delta可能遗漏，必须阻断无损切换声明**。源版本ETag不能当校验唯一依据，迁移计算SHA-256和字节长度作为完整性证据。

## 4. 迁移数据完整性与差异

| 项目 | 必须无损 | 可明确报告差异 | 不能自动静默转换 |
|---|---|---|---|
| 源对象字节/长度/SHA-256 | 是，100%目标对象核验 | 无 | 数据转码 |
| 历史Version/当前Head/DeleteMarker顺序 | 是（公共逻辑目录） | 物理版本串变化必须明示 | 历史丢失/顺序改变 |
| tags/user metadata/content type | 是（受支持并声明字段） | provider非法字符/size差异需阻断或签署 | 抛弃用户metadata |
| Retention/LegalHold/Governance/Compliance | 是，对要求WORM的桶 | 无法满足则Blocked | 迁到无法证明WORM的后端 |
| IAM/Bucket Policies/ACL | 逻辑网关策略保持一致 | 上游具体策略不完全等价须列明 | 无授权默认公开 |
| SSE/KMS/数据驻留 | 是（受安全政策约束） | KMS key rewrap与地区差异可描述 | 明文落盘/越境转移 |
| ETag、原Provider physical VersionId | 不保证字节一致 | 映射关系记录可查询 | 将physical串假称逻辑VersionId |

## 5. 切换事务、并发与回滚

```
1. assert source route epoch = E and target copy barrier >= source published watermark
2. WriteFence：禁止新写/等待inflight或将已接收操作按可靠日志重放至target
3. 检查 target每逻辑VersionId对应physical placement verified、Retention等价
4. MetaRaft CAS (bucket.route_epoch=E+1, active_backend=target)
5. 停止源写授权/禁止旁路，路由新读写至target；观测数据和错误率
6. 保持旧源安全只读或受控保护，禁止源物理删除直到审批结束
```

**写入栅栏**必须考虑已返回2xx但事件尚未复制的写入；如果无法完整检测，将发布阻断、撤销切换。切换后确认了**新目标写入**，若需回滚到旧源，必须先反向增量同步并校验后再次写栅栏、路由CAS更新；**不能直接把开关拨回旧源**。允许自动回滚的唯一前提是没有新确认写入或已验证反向完全追平。

## 6. 复制模式与容灾RPO

跨后端复制默认`single-writer async replication`，事件由Raft authoritative目录按全序生成，再投递worker并幂等落目标；删除标记与Version需要保序。Lag计算由`current_master_committed_watermark - replica_verified_watermark`及时间维度表达，不能仅用队列长度表示RPO。Replica GET仅在满足读目标请求的一致性约束时切换。读故障转移与主从提升分开处理，提升新写主必须fence旧主、严格检查数据驻留/WORM和新的routing_epoch。

## 7. HA/DR拓扑设计

同站元数据Raft集群3/5 voter跨故障域，API水平扩展；数据碎片跨故障域，WORM与KMS依独立高可用服务。异地DR：定期加密Raft snapshot、逐版本manifest、对象副本、路由映射、Git授权链接、审计验证证据；DR恢复先恢复密钥/身份/metadata，再验证实际object bytes，后开放读取/写入。GitGit Core本身故障或Ref真相缺失时，依旧可读取已有获授权LFS对象，但不能安全执行Git GC/新的pre-receive。

### 7.1 候选SLO承诺拆分

| 指标 | SRS候选 | 必须注明 |
|---|---|---|
| API可用率 | ≥99.95%/月 | 读/写分项、错误计数、故障域、维护口径 |
| 已ACK对象数据完整 | 声明容错内无丢失 | EC分片durability receipts、meta quorum |
| RTO | ≤60分钟 | KMS/metadata/DR replica依赖可用 |
| RPO | ≤15分钟异步跨站 | 会损失的故障窗口明确，复制lag可追踪 |
| 六节点B1 | 4+2，可容忍单AZ失2个节点后的读取 | **默认不支持在AZ失效时继续完整冗余新写** |

## 8. 切换失败与恢复动作

| 故障 | 默认动作 | 风险/证据 |
|---|---|---|
| Inventory缺历史版本 | 阻止切换 | 差异清单：不可恢复版本 |
| 源403/WORM权限不足 | Blocked，审批后修正scope | 不允许跳过受保护版本 |
| 目标429/503 | 退避+租户配额+断点重试 | 避免雪崩放大 |
| Copy校验SHA不符 | 删除/隔离未发布目标、重新复制 | 不能将目标标记Verified |
| 切换时突发写入 | Fence旧epoch并等待inflight确认 | 已ACK写入必须进入final delta |
| Cutover后目标发生写入 | 旧源不是可直接回滚主 | 先reverse catchup并再次Fence |
| 目标突然无法访问 | 按已验证一致性回源只读/停止写 | 保证不写出“双主” |
| 审计sink断开 | Outbox堆积并报警，高风险操作有Policy Gate | 不丢业务证据 |

## 9. 运营与成本

报告区分源/目标数据规模、Multipart API请求、PUT/GET/DELETE costs、出站流量、版本冗余、临时双写窗口、SCRUB成本。业务迁移必须由管理面进行Dry-run和权限审批；失败后能按每桶/Key/Version追踪原因，不以“迁移任务成功”代替对象级验证结果。

## 10. DR演练（按季度）

准备版本、WORM、Git引用、最近写入的测试数据集和KMS材料；执行杀节点/切region/失身份服务/恢复snapshot/校验字节/回切演练，记录各步UTC时间戳、RPO/RTO实际值、证据散列及损坏/未完成对象数量。未通过RTO/RPO目标则阻断E1验收，不靠手工填写通过。
