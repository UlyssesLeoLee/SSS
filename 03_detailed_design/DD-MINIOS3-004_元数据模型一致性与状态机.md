# DD-MINIOS3-004 — 元数据模型一致性与状态机

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：Raft catalog、KV索引、跨分片LIST、版本引用


## 1. 权威元数据KeySpace设计

本产品使用Raft状态机后面的**有序KV**而非独立SQL作为对象目录真相；以下为逻辑编码，生产实现须定义prefix-free长度编码、big-endian整数、schema_version+CRC，避免key碰撞。

| Prefix（概念） | Key成分 | Value要点 | 强一致操作 |
|---|---|---|---|
| `g/tenant/` | tenant_id | 状态、quota、区域策略 | policy epoch CAS |
| `g/bucket_name/` | exact_name | bucket_id + tenant_id | global uniqueness CAS |
| `g/bucket/` | bucket_id | routeEpoch、versioning、lock config | route CAS |
| `h/` | bucket_id+encoded_key | latestVersion、deleteMarker、head_seq | insert/update CAS |
| `v/` | bucket_id+encoded_key+commit_seq | logicalVersionId、checksums、Lock、flags | append-only |
| `p/` | logical_version_id+generation | placement backend/version/manifest | generation替换 |
| `c/` | scope_id+OID+size | CAS manifest state | immutable OID mapping |
| `l/` | repo_id+OID | Repository grant link | grants revisions |
| `u/` | upload_id+part_no | partReceipt / etag / expiry | MPU CAS |
| `i/` | scope+operation_id | payload digest/outcome/TTL | 幂等查询 |
| `j/` | job_id | lease + checkpoint + phase + approval | job step CAS |
| `o/` | group_id+log_index+event_id | audit outbox | 与业务txn同行 |
| `r/` | repo_id+snapshot_id | Git reachability watermark | GC check |

值的`schema_version`使用向前可读策略；发布数据不更新原文，只添加新Version行并原子更新Head。快照/WAL中不可出现AccessKey密文原值、媒体原文或外部凭据。

## 2. 状态机命令与事务边界

| Command | Preconditions | Atomic write-set | Error |
|---|---|---|---|
| `PreparePut` | auth/policy valid, expected head, quota ok | reservation+ticket+idem | Conflict/QuotaExceeded |
| `CommitPut` | ticket TTL, epoch, receipts, checksum valid | version+head+placement+quota+outbox+idem | Expired/BadDigest/Conflict |
| `AbortPut` | ticket exists, not committed | release reservation + tombstone ticket | Conflict |
| `DeleteVersion` | correct version, lock allows, refs check | delete/tombstone + outbox | RetentionViolation |
| `PutCAS` | verified sha+size, authorized scope | CAS manifest+repo grant | Collision/AccessDenied |
| `CommitRouteEpoch` | approved job, fenced old epoch, target verified | route epoch + placement pointer + outbox | StaleEpoch/Blocked |
| `MarkGCCandidate` | signed Git watermark complete, no holds | candidate+grace_until | GuardRejected |
| `FinalizePurge` | second fresh validation, quorum, retention | tombstone/physical-delete task | GuardRejected |

**元数据提交与物理Shard存储不能用同一个Raft事务**。必须以已持久化receipt作为Raft发布前置，保留orphan补偿机制。

## 3. Raft读写与leader变更

- 使用一致性协议库版本锁定后的确认语义；`apply_index`不等于数据切片fsync，Meta Commit只负责目录发布。
- 写入口须附term/epoch、客户端幂等标识和 expected_seq；失去leader仅重定向/503，不能在Follower本地假提交。
- 强读使用read-index/barrier并在已apply索引上读取；Leader换届后对比`last_applied`/`committed`进行安全读。
- Snapshot编码包含membership、applied index、policy/route epoch、目录状态、outbox backlog、操作记录；校验snapshot hash，恢复后执行manifest与物理存储可读性核验。

## 4. 全Bucket LIST一致性决议

**默认方案 DD-DEC-04：每Bucket单一范围组**。即使跨租户支持多个组，单Bucket键空间由该组的MVCC有序快照保证；跨多个组的Bucket级快照暂不启用（REV-010）。此方案是安全设计选择，但可能无法达到1亿key性能目标；按NFR-PER-003真实压测评审后再扩展，不得默默改为最终一致。

具体流程：ReadIndex→AcquireSnapshot(index,ttl)→Seek(prefix+start_after)→产生含snapshot id、last key、`filters_hash`的AEAD token→下页复用同一snapshot；改变delimiter、prefix或principal的token无效。GC/Compaction不可删仍被有效token和Git snapshot引用的历史索引。Snapshot有效期在契约中定义并纳入容量预算。

## 5. 故障时数据状态与校验

| 故障注入点 | Object可见性 | 清理/恢复 | 测试断言 |
|---|---|---|---|
| prepare前故障 | 不可见 | 无 | quota不增 |
| prepare后故障 | 不可见 | TTL释放reservation | 无半对象 |
| data durable未commit | 不可见 | 标记orphan,延后回收 | 原始片仍可识别 |
| Raft commit后ACK前断线 | 已可见 | 通过op_id还原结果 | 只有一个logical version |
| Raft follower落后 | 不允许陈旧强读 | read-index等待/503 | 已ACK内容不回退 |
| 旧leader继续写 | fencing拒绝 | 重试新term | 同key不产生两个Head |
| WAL破损/快照恢复 | 未验证时隔离 | 恢复校验/hash | 不把数据丢失报成404 |

## 6. 导出、升级、回收

Metadata schema前进采用双读可选字段→新写→全量回填→校验→门禁切换；不允许滚动期间由N+1写入N完全不能理解的必需字段。GC仲裁器读取Git refs snapshot、WORM、LegalHold、迁移双placement、审计保留、引用计数、活跃upload；任一未知进入Blocked。

## 7. 形式化/性质测试目标

模型命令列表`Prepare/Receive/Commit/Delete/RouteCutover/Lock/Gc/Failover`，性质：never ACK without durable receipt、no committed version lost after single tolerated failure、no stale route accepted、no purge when protected、no duplicate version on op_id replay、no cross tenant exposure。结合TLA+/Apalache或Rust状态机proptest择一执行，形式化探索不得被“用了Raft库”取代。
