# IF-MINIOS3-001 — 外部接口设计与协议边界

## 1. 外部接口清单
| API域 | 代表操作 | 输入合同 | 关键错误 | 权威方 |
|---|---|---|---|---|
| GitGit Native | prepare/upload, commit/ref CAS, checkout | `gitgit_native_openapi.yaml` | 409 RefConflict, 412 EpochMismatch | GitGit Core |
| MinIOS3 Native | Chunk Put, Seal Manifest, durable receipt, range read | `gitgit_native_contract.json`; `content_manifest.schema.json` | MissingChunk, HashMismatch, ReceiptExpired | MinIOS3 CAS |
| S3 Gateway | bucket/object/multipart/version/lock API | S3兼容矩阵+签名规范 | AccessDenied, InvalidRequest, NoSuchVersion | MinIOS3 Gateway |
| Admin | Tenant/Bucket/route/migration/health | `admin_api_openapi.yaml` | PolicyDeny, CutoverUnsafe | ControlPlane |
| Optional LFS | batch/verify/lock | `legacy_git_lfs_adapter_contract.json` | explicit incompatible feature | Optional Adapter |

## 2. 互操作约束
1. S3物理VersionId不是GitGit CommitId/ManifestId；后端迁移必须持久化映射。
2. 接口必须按schemaVersion协商升级；客户端未知字段采用明确兼容策略，不静默丢失Retention/ACL语义。
3. SigV4、签名时钟、Canonical URI、Query与Payload digest独立做负例；multipart ETag不得无条件解释为MD5。
4. 幂等键适用于可安全重复命令；对已进入cutover或不可逆锁定状态需返回状态查询链接，不无条件重放写入。
5. 禁用Git LFS与标准Git兼容服务不得阻断GitGit原生业务。

## 3. 读写一致性
GitGit Ref CAS线性化于GitGit权威元数据事务；MinIOS3 DurabilityReceipt表示被验证的可靠持久化语义；跨服务使用事务协调/Outbox/重放/补偿，不假设跨服务ACID。
