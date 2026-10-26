# DD-MINIOS3-006 — GitGit事务门禁与安全回收（置换旧GitLFS主路径）

> v2.0 · 对应FR-GG-003/005/006/016/020、FR-MV-019/028、FR-GIT-005/009/010/013

## 1. 职责/模块

`gitgit-commit-coordinator`持有Commit/Ref事务；`minios3-receipt-service`对清单及分片可靠持久化签署Receipt；`gitgit-outbox`传播已提交事件；`minios3-gc-coordinator`执行二阶段回收。不得调用外部GitHook作为必要环节。

## 2. 提交状态机

`DRAFT → UPLOADING → MANIFEST_SEALED → PREPARED → COMMIT_STORED → REF_PUBLISHED → OUTBOX_RECONCILED`；任一前置校验失败→`ABORTED`或`RECOVERABLE`（按是否产生已确认CAS）。服务不得标记已ACK的Ref为失败后悄悄撤回；重试由`txn_id + expected_old_ref + manifest_digest`识别原操作。

1. 先将全部Chunk落盘，验证SHA-256/长度、持久化收据和PlacementEpoch。
2. ManifestSealed必须不可变，提交准备票据绑定`repo, actor, base_ref_revision, ref_name, commit_digest, manifest_ids, authz_epoch, expiry`。
3. GitGit服务在单个事务完成 `CommitIndex.PutIfAbsent` 与 `Ref.CompareAndSwap` + outbox事件，返回新revision与event_id；GC必须在PREPARED期间保留关联内容。
4. 在Ref CAS失败时不自动复用过期ticket；未引用但已持久Chunk进入宽限期保护，不提前删除。
5. 消费者对Outbox按 `(repo_id, stream_seq)` 幂等重建索引，签名快照和watermark定期校准。

## 3. 原子性边界

GitGit的Ref CAS和Outbox应在同一权威事务中提交；MinIOS3的Chunk durable receipt在此前独立完成，不可宣称跨系统ACID。网络在Ref已提交但响应丢失时，调用方通过`operation_id`查事务状态，禁止直接重新创建不同Commit。

## 4. 安全GC

获取GitGit签名全图可达性快照（包含Tag、protected branch、Review、OfflineSync租约、复制/迁移恢复点），若存在缺失或水位过期则FailClosed；先以Raft更新tombstone候选，延后并再次核验全部链接及WORM后才物理删除Chunk。对象入保留的新引用与回收的竞争由`gc_epoch`栅栏裁决，删除前最后复验。GC日志与拒绝原因可查询。

## 5. 错误及重试

`CONFLICT_REF`(409)、`MISSING_MANIFEST`(409)、`UNSEALED_CHUNK`(409)、`INVALID_RECEIPT`(422)、`POLICY_EPOCH_CHANGED`(412)、`OUTBOX_LAG`(503)、`GC_REACHABILITY_UNKNOWN`(423)。可重试冲突必须由客户端重新获取RefRevision，不允许盲目覆盖。

## 6. 测试设计关联

UT：事务模型及Ref CAS性质测试；IT：真实Rust GitGit+MinIOS3双方崩溃窗口/Outbox重放；ST：100GB/TB历史仓库恢复+GC压力、0个缺块已ACK提交。
