# DD-MINIOS3-002 — Rust模块分层与内部接口

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：全部Rust crates和二进制


## 1. 关键领域类型设计

```rust
// 概念接口：展示类型边界，非可直接编译的实现代码。
struct TenantId(Uuid);
struct BucketId(Uuid);
struct LogicalVersionId(String);
struct RouteEpoch(u64);
struct PolicyEpoch(u64);
struct OperationId(Uuid);
struct LfsOid([u8; 32]);
struct VerifiedManifest { raw_sha256: [u8;32], size: u64, receipts: Vec<Receipt>, epoch: RouteEpoch }
enum DomainError {
  InvalidInput, Unauthenticated, AccessDenied, PreconditionFailed,
  RetentionViolation, CapabilityUnavailable, QuotaExceeded, MetaUnavailable,
  BackendUnavailable, IntegrityFailure, Conflict, AlreadyCommitted,
}
```

必须使用不可变强类型ID；`SecretBytes`不实现Debug，Drop时零化机密缓存（仍不能保证所有OS拷贝被清零）；路径Key不借用`std::path::Path`处理。

## 2. Service接口及输入输出

| Trait（提供方） | 方法及输入 | 返回 | 前置条件 | 后置条件 |
|---|---|---|---|---|
| `AuthenticationService` | `authenticate(raw_headers, original_uri)` | Principal + signature context | 原始URI未被代理改写 | 失败不运行授权与存储 |
| `PolicyEngine` | `authorize(action, resource, condition, policy_epoch)` | Permit + audit decision | principal完整、资源Tenant确定 | Deny优先；epoch陈旧拒绝 |
| `Catalog` | `prepare_put(ObjectIntent)` | PutTicket + quotaReservation | 条件头在强一致head上验证 | 尚无公开对象版本 |
| `PlacementEngine` | `place(intent, fault_domains)` | PlacementPlan | 能力/容量已验证 | 不满足域约束拒绝 |
| `StorageAdapter` | `put_immutable(ticket, ByteStream)` | VerifiedReceipt | route epoch+ticket有效 | 数据已持久，可独立核验 |
| `Catalog` | `commit_put(ticket, receipt, op_id)` | LogicalVersion + logIndex | 再次验证epoch/retention/配额 | 原子更新head/version/outbox |
| `Catalog` | `get_consistent(bucket,key,selector)` | ReadPlan | 当前read barrier可用 | 只有Committed版本可读 |
| `GitBridge` | `prepare_ref(TrustedDiff)` | ExpiringTicket | 服务端解析tree，已确认所有OID | 不替GitGit Core修改Ref |
| `GCGuard` | `evaluate_purge(candidate, watermark)` | Permit/Block reason | Git ref快照完整且签名有效 | 不能越过WORM/legalHold |
| `MigrationCoordinator` | `advance(job,expected_epoch)` | CheckpointState | 当前lease持有人 | CAS状态前进/幂等 |
| `AuditOutbox` | `append_with(txn,Event)` | EventId | 目录同一Raft事务 | 成功写入能重放 |

## 3. 异步消息和资源预算

- Handler仅处理协议解析、鉴权与命令调用；所有大对象是`Stream<Result<Bytes,E>>`，使用背压发往每后端有界队列。
- `Semaphore` 的budget以tenant、后端、操作级分层：全局允许数≥各tenant总担保份额；迁移/preview为可抢占低优先级，Git读写优先。
- Timeout由调用层传下统一`RequestDeadline`；重试前检查`idempotency_key`以及Raft结果而非重发不明确提交。
- CPU密集型EC编码/媒体解码放置独立blocking pool/worker进程；峰值RSS需可用B1监控证实。

## 4. 错误、日志与metrics

每层只暴露`ErrorKind`和可重试性，外层翻译为协议错误；`audit_id`与`trace_id`在每个error中传播。结构化日志字段包含`tenant_hash`而非敏感用户名明文、`backend_id`、`raft_group`、`route_epoch`、`operation_id`；不得记录token/Authorization/SecretKey。

| 故障 | 是否重试 | 外部行为 | 指标 |
|---|---|---|---|
| 用户签名错 | 否 | S3 403 / SignatureDoesNotMatch | auth_failure_total |
| 元数据多数派丢失 | 有界退避 | S3 503，不提供虚假线性化读 | meta_quorum_unavailable |
| Provider 429 | Retry-After+抖动 | 根据预算429/503 | backend_throttle_total |
| checksum不匹配 | 否，标记异常 | 400 BadDigest或5xx integrity读错 | integrity_failure_total |
| KMS被拒 | 不明文回退 | AccessDenied/503按根因 | kms_denied_total |
| 旧route epoch | 重查后再次授权 | 409/503且不会双主写 | route_fence_reject_total |

## 5. 模块间RPC契约

proto定义见`02_contracts/storage_internal.proto`。内部调用必须携带：`api_version`, `request_id`, `principal_service`, `tenant_id`, `operation_id`, `expected_route_epoch`, `deadline_ms`, `manifest_digest`；敏感请求必须双向mTLS。proto字段不随版本更改含义，弃用采用新增字段+版本协商。

## 6. Rust静态与UT门禁

`cargo fmt --check`、`clippy -D warnings`、`cargo test --workspace`、SCA/license/SBOM、MSRV核验、无未审unsafe（unsafe代码清单及独立审核），`cargo llvm-cov`按模块及变更范围测得核心路径目标≥90%。P0状态机必须配proptest/model-based tests + mutation/fuzz，并将所有状态转移写进UT计划。

## 7. 失败注入点

FI-R01：prepare后退出；FI-R02：第5片fsync失败；FI-R03：Raft Commit返回前断链；FI-R04：commit成功但HTTP断开；FI-R05：policy epoch撤销与GET并发；FI-R06：migration lease超时；FI-R07：Git event重放乱序；FI-R08：operator二次批准。每一故障有唯一`op_id`进行前后恢复比对。
