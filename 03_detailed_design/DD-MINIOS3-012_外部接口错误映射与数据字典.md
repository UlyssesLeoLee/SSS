# DD-MINIOS3-012 — 外部接口错误映射与数据字典

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：API wire格式、通用返回、数据规范与兼容边界


## 1. 接口详细清单

| I/F ID | 协议 | 内容类型 | 认证 | 操作例 | 必需测试 |
|---|---|---|---|---|---|
| IFD-01 | S3 REST | XML+字节 | SigV4/STS | PUT/GET/LIST/Version/MPU | canonical+xml+header+SDK |
| IFD-02 | Git LFS兼容 Batch | Git LFS兼容 JSON | Scoped git token | upload/download/verify | 标准git-lfs client |
| IFD-03 | Git Locking | Git LFS兼容 JSON | Repo scope | Create/List/Verify/Unlock | bypass客户端hook仍拒绝 |
| IFD-04 | Git Hook | JSON mTLS | GitService | prepare/confirm Ref | GitGit Core原子Ref CAS |
| IFD-05 | Git Events | JSON mTLS+signature | GitService | event+snapshot | 重放/丢消息/乱序 |
| IFD-06 | Admin | OpenAPI 3.1 JSON | OIDC/MFA | Bucket/Migration/Audit | Role/Idempotency-Key |
| IFD-07 | Native Storage | gRPC proto3 | mTLS/ServiceCert | PutShard/GetShard | FSYNC+epoch+receipt |
| IFD-08 | Meta Node | Raft RPC | mTLS | propose/read-index/snapshot | leader/follower切换 |
| IFD-09 | S3 upstream | HTTPS SigV4 | Scoped provider AK | versioned object/Multipart | 两类实际供应商 |
| IFD-10 | Audit sink | OTLP/JSON | mTLS | at-least-once events | 重放不双记+不可变收据 |

## 2. S3错误映射合同（具体code必须以受测兼容清单落签）

| 内部条件 | HTTP | S3 XML Code | 注意 |
|---|---|---|---|
| `Unauthenticated`/坏签名 | 403 | SignatureDoesNotMatch/AccessDenied | 错误码按签名根因 |
| `AccessDenied`/Retention保护 | 403 | AccessDenied | 不能泄漏跨租户Key存在 |
| `NoSuchBucket` | 404 | NoSuchBucket | 权限/隐匿策略需实测 |
| `NoSuchKey` | 404 | NoSuchKey | 不把物理损坏映射为404 |
| `QuotaExceeded` | 403/429 | QuotaExceeded或SlowDown（契约冻结） | 内外错误码差异需签名 |
| `BadDigest` | 400 | BadDigest | 不能发布部分对象 |
| `PreconditionFailed` | 412 | PreconditionFailed | 重验head sequence |
| `InvalidRange` | 416 | InvalidRange | Content-Range按API矩阵 |
| `NoSuchUpload` | 404 | NoSuchUpload | Complete/Abort并发差异需测 |
| `MetaUnavailable` | 503 | ServiceUnavailable | quorum不足不得假ACK |
| `IntegrityFailure` | 500/503 | InternalError/ServiceUnavailable | 记录Sev与隔离 |

响应统一生成`x-amz-request-id`，管理员另用`X-Request-Id`，SDK错误必须可按RequestId定位trace。API不得泄漏内部provider bucket/key path、堆栈和密钥。

## 3. 字段定义

| 字段 | 类型/限制 | 校验/序列化 | 安全级别 |
|---|---|---|---|
| `tenant_id` | ULID/UUID概念 | 完整不可猜，非Trust source授权 | 内部 |
| `bucket_name` | S3合法名称及最大长限制 | 单Endpoint全局唯一；IDN/TLS边界 | 公开经授权 |
| `object_key` | S3允许的字节串 | byte-preserving，禁止文件路径归一 | 公开经授权 |
| `logical_version_id` | opaque string | 版本化桶稳定，可经URL定位 | 公开经授权 |
| `physical_version_id` | opaque string | 私有，不得当公共版本 | 机密业务元数据 |
| `lfs_oid` | 32字节SHA-256 | 64字节hex lowercase，上传全字节对照 | 有授权时可公开 |
| `checksum_raw_sha256` | 32字节 | 对原始未变更数据计算 | 授权元数据 |
| `route_epoch/policy_epoch` | u64 | 单调增+CAS，缓存栅栏 | 内部 |
| `retention_until` | UTC Timestamp | 可信时钟；只在合法模式允许延长/解除 | 敏感 |
| `manifest_digest` | SHA-256 | canonical serialization明确hash域 | 内部 |
| `audit_event_id` | UUIDv7/唯一ID | at-least-once投递去重 | 审计 |

## 4. API契约文件说明

本包的`02_contracts/admin_api_openapi.yaml`直接沿用BD接口基线，不擅自声称其完整覆盖全部Admin API；新增`storage_internal.proto`和`git_bridge_contract.json`均为**设计契约示意**，用于详细设计评审，尚未运行代码生成器/官方协议测试。完整S3逐操作兼容状态沿用`compatibility_manifest.json`。

## 5. 通讯限制/防护

所有流式上传有总长度、header-count、header-bytes、part数、连接数、backend queue长度、socket deadline上限；值根据压测校准并入ConfigSchema。拒绝时包含不敏感追踪ID，遇到不支持的AWS checksum/STREAMING签名不得静默替换为UNSIGNED。预签名仅在网关可撤权域内承诺即时撤销，外部Provider签名不能受本网关控制。
