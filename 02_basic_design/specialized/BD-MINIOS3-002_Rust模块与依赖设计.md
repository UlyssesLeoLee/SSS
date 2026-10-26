# BD-MINIOS3-002 — Rust模块、分层与技术选型基本设计

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 上级设计：BD-MINIOS3-001。状态：待详细设计/依赖锁定。核心原则：Rust原生、不复制MinIO源码、业务行为通过trait约束。

## 1. 技术选型表（均为候选，执行时核验最新版本/许可）

| 关注点 | 候选 | 选择依据 | 不使用/警惕 |
|---|---|---|---|
| 运行时 | Tokio | 成熟的异步I/O、任务调度、结构化超时/取消 | 不使用无限spawn或无限channel |
| HTTP | Axum + hyper + tower | S3/XML REST、管理JSON、Middleware、流式body | 禁止把S3错误统一包装为JSON |
| 内部RPC | tonic/prost | mTLS、明确proto版本、双向流可控 | gRPC不替代标准S3 HTTP接口 |
| TLS | rustls | 可复现的TLS栈 | 禁用弱算法/未验证证书 |
| 签名 | 独立`protocol-s3`/现有成熟库可参考但必须兼容测例 | 精确处理canonical request/query/headers/streaming | 不能先URI解码再签名或者信任代理头 |
| Backend S3 | aws-sdk-s3（Rust） | 对接AWS与S3兼容Endpoint、重试策略可控 | 并非自动提供对外S3兼容服务 |
| 元数据共识 | openraft 候选 | Raft复制、membership与snapshot设计 | Raft只是共识框架，不自动保证业务数据不丢 |
| 本地有序KV | RocksDB-compatible LSM（具体crate待批准） | ordered iteration、快照、WAL/压缩 | Native依赖/跨架构编译和许可证审核 |
| 纠删编码 | Reed-Solomon实现（crate待确认） | k+m、流式切片、硬件优化空间 | 对齐、SIMD、ABI、许可证须验证 |
| 哈希/校验 | SHA-256、CRC32C | LFS兼容OID/数据完整性分别使用 | 不把ETag强制设为SHA或MD5 |
| 加密 | RustCrypto AEAD候选 + KMS client | 按块加密与信封密钥 | nonce复用、缓存密钥过长 |
| 配置 | serde, config/toml/yaml adapters | schema校验与版本化 | 环境变量不可覆盖安全限制导致降级 |
| 指标/日志 | tracing、OpenTelemetry、Prometheus exporter | 统一trace_id及可观测性 | 高基数Key/OID写入指标label |
| 媒体解析 | Rust worker+隔离执行外部工具（审查后） | 分离故障、适配复杂格式 | 不允许运行在S3进程、无需自研解码器 |
| 浏览器UI | 轻量Web前端（推荐TS） | Rust后端API稳定、安全与UI解耦 | Rust要求是服务端核心，不强制前端WASM |

**Cargo治理**：workspace依赖集中声明；首次详细设计锁定Rust toolchain和 crate精确版本；commit Cargo.lock；许可证allowlist、SCA扫描、cargo audit/deny、SBOM、签名发布物、可复现构建、跨amd64/arm64 CI。

## 2. Domain crate责任与禁止的依赖

```text
protocol-s3, protocol-lfs, admin-contract
              ↓
            domain ← authz / security
              ↑
 metadata/storage-api/git-integration/migration/replication/media
              ↑
  executable composition root (tower layers, tracing, DI)
```

`domain`只有规则/值对象/状态机定义与纯接口，不依赖web框架或物理存储。`protocol-*`只处理HTTP/JWT/SigV4与domain的转换。`metadata`实现权威目录接口；`storage-native/storage-s3`实现后端trait。所有跨模块调用使用强类型 `TenantId`, `BucketId`, `ObjectVersionId`, `OidSha256`, `RouteEpoch`, `IdempotencyId` 防止错误串用。

## 3. 核心接口形态（Rust trait伪签名，非可编译源码）

```rust
// 仅说明边界与语义；关联类型及async生命周期由详细设计确定
trait ObjectCatalog {
    async fn prepare_put(&self, ctx: AuthzContext, key: ObjectKey, req: PutIntent)
        -> Result<PutTicket, DomainError>;
    async fn commit_put(&self, ticket: PutTicket, durability: VerifiedManifest)
        -> Result<CommittedVersion, DomainError>;
    async fn get_at(&self, ctx: AuthzContext, key: ObjectKey, ver: VersionSelector)
        -> Result<VisibleManifest, DomainError>;
    async fn list_linearizable(&self, query: ListQuery)
        -> Result<KeyPage, DomainError>;
}
trait Backend {
    fn capability(&self) -> BackendCapability;  // 权威配置来源+检测证据
    async fn put_immutable(&self, intent: PhysicalIntent, data: ByteStream)
        -> Result<PhysicalReceipt, BackendError>;
    async fn get_range(&self, placement: Placement, range: ByteRange)
        -> Result<ByteStream, BackendError>;
}
trait GitBridge {
    async fn verify_push(&self, request: RefUpdateIntent)
        -> Result<ShortLivedRefTicket, GitError>;
    async fn ingest_ref_event(&self, event: SignedRefEvent)
        -> Result<EventAck, GitError>;
}
```

契约关键：`prepare_put`预留配额并签发含存储策略/路由epoch的票据，不能发布可读版本；`put_immutable`返回经持久化检查的不可变物理receipt；`commit_put`检查epoch、摘要、保留与幂等后在Raft提交发布；`get_at`在适当read barrier后读取；`Backend`的capability不能依赖未经验证的宣传参数。

## 4. 错误分类与跨层规则

| 类别 | Rust内部错误类型 | S3映射 | Admin映射 | 重试性 |
|---|---|---|---|---|
| 客户端不合法 | `InvalidArgument` | `InvalidArgument/InvalidRequest` | 400 | 否 |
| 凭据/权限 | `Unauthenticated/AccessDenied` | `SignatureDoesNotMatch/AccessDenied` | 401/403 | 否 |
| 目标不存在 | `MissingResource` | `NoSuchKey/NoSuchBucket` | 404 | 否 |
| 乐观冲突 | `PreconditionFailed/ConcurrentUpdate` | `PreconditionFailed/OperationAborted` | 409/412 | 可能 |
| 保留限制 | `RetentionViolation` | 403 AccessDenied等兼容项 | 409/403 | 否 |
| 容量不足 | `QuotaExceeded/CapacityLimited` | `SlowDown/InsufficientStorage`（矩阵确认） | 429/507 | 待条件变化 |
| 元数据quorum | `MetaUnavailable` | 503 ServiceUnavailable | 503 | 是，退避 |
| 物理校验失败 | `IntegrityFailure` | 5xx | 500/503 | 视恢复 |
| Provider 429/5xx | `BackendThrottled/Unavailable` | 已认定的S3兼容错误 | 429/503 | 是、有界 |

不可将所有Rust内部错误直接通过`Debug`写到HTTP响应；不输出密钥、内部Bucket映射、路径、stack traces。Error映射契约测试为发布门禁。

## 5. 流式I/O、安全取消与调度

- 每个请求设置输入body上限（非对象字节上限）、连接/请求超时、逐租户并发Budget和出站带宽配额。大对象使用stream分页读取；不允许`collect()`把20GiB装入内存。
- 有界channel、chunk pool、`CancellationToken`，客户端断开后等待完成或可靠释放临时资源；已进入提交状态不可因为HTTP断开强行撤销已提交物理对象。
- 不对Multipart Complete或迁移提交执行盲重试；先检查operation_id、元数据提交索引、目标physical receipt。
- Metadata Raft提交日志只能包含小元数据及摘要，不包含用户文件字节、访问令牌或秘密。
- CPU密集型EC编码、SHA处理、预览处理分离独立调度池/进程；任务预算含RAM、fd、IOPS、CPU。

## 6. 分层测试与质量Gate

`domain`属性测试（版本不可逆、保留单调、策略默认拒绝）；`protocol-s3`生成canonical/fuzz（空Key、双重编码、Unicode、长header）；`metadata`proptest+模型检查（quorum/read-barrier）；`storage-native`坏盘及校验块；`storage-s3`mock+真AWS/MinIO回归；`git-integration`标准git-lfs客户端；`migration`kill/replay；最终B1性能+chaos+DR。风险最高的模块以变更为单位强制审核；覆盖率≥90%目标，关键分支需要100%设计用例。

## 7. 扩展性与插件模型

存储后端扩展以Rust特性门控编译或独立签名sidecar方式加入。生产默认禁用运行时动态执行未经审核的WASM/native插件；若必须热插拔，需独立进程、版本化RPC、资源配额、双重许可审核。媒体预览插件固定独立沙盒；不得直连原生数据节点或持有核心KMS master key。

## 8. 待详细设计问题

网络读超时/keepalive、Metadata snapshot store接口、Stripe大小、Raft membership reconfiguration、AES-GCM nonce派生、拆分meta range的原子迁移步骤、S3 SigV4 streaming签名支持子集与详细错误码，在DD中逐项定稿并附契约/故障测试。
