# ID-MINIOS3-001 — 内部设计（内部設計）物理数据/事务/状态模型

## 1. Rust元数据逻辑Key空间（待定物理编码）
| 命名空间 | 键形状 | 值核心字段 | 约束 |
|---|---|---|---|
| `repo/` | `{tenant}/{repo}` | policy_revision, next_sequence | tenant isolation |
| `commit/` | `{repo}/{commitId}` | parents, treeId, author, receiptsDigest | immutable |
| `ref/` | `{repo}/{refName}` | commitId, revision, policyEpoch | atomic CAS |
| `asset/` | `{repo}/{assetId}/{revision}` | manifestId, formatMeta, parentRevision | immutable revision |
| `manifest/` | `{domain}/{manifestId}` | chunks, length, sha256, state | sealed only when complete |
| `chunk/` | `{domain}/{chunkDigest}` | size, placement, integrity, retentionHints | immutable bytes |
| `receipt/` | `{txn}/{manifestId}` | signedReceipt, durableEpoch, expiresAt | cryptographic verify |
| `outbox/` | `{repo}/{sequence}` | eventType, committedRefRevision | same transaction as RefCAS |
| `gc/` | `{tenant}/{epoch}/{candidate}` | reachabilityProof, tombstone, graceDeadline | double-check |
| `route/` | `{bucket}/{epoch}` | backendId, logicalVersionMapping | epoch fencing |

## 2. 状态机
- `Upload`: Initiated→Receiving→Sealing→Durable→Expired/Rejected。只有Durable可获Receipt。
- `GitCommit`: Prepared→Checked→Committed/Conflict；Committed可见性与Ref CAS原子化。超时由查询接口裁决，不重试生成另一个结果。
- `Object`: Staged→Committed→Protected/EligibleForGC→Quarantined→Deleted；受Retention保护禁止进入Deleted。
- `Migration`: Planned→Scanning→Copying→Verifying→Quiescing→Cutover→Monitoring→Completed/RollbackSync→RolledBack；根据epoch拒绝旧写者。

## 3. 关键不变量与执行处
INV-01已提交Ref不得指向不存在的Commit；INV-02已提交Commit的每个Manifest必须为Sealed+Durable；INV-03Manifest chunk顺序、offset连续无重叠；INV-04租户隔离的Dedup安全域互不泄漏；INV-05 GC删除前重新读取权威reachability与Retention；INV-06 后端切换不能丢ACK的版本；INV-07 不可由非合规配置移除WORM保护。

## 4. 事务和崩溃窗口
一次GitGit元数据事务包含Commit写入、Ref expected revision比较交换及Outbox append。跨MinIOS3的Receipt检查发生在GitGit提交前，失败时旧Ref不改变；Crash后的Outbox replay按eventId幂等。物理GC使用两阶段tombstone、grace和epoch二次查验，宁可暂时泄露空间也不误删历史。

## 5. 物理化决策尚未冻结
redb key编码/索引、OpenRaft日志fsync策略、快照校验、数据分片一致性及KMS调用耗时预算都属于详细设计+PoC要证明的对象，不预先宣称达到性能目标。
