# BD-MINIOS3-003 — 外部接口与通信协议基本设计

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 主要对应：FR-S3-001~019、FR-GIT-001~014、FR-XCHG-001/003/004、FR-SEC-001/003/006，NFR-MIG-004。

## 1. 协议族与认证

| ID | 接口类型 | 传输格式 | 鉴权 | 版本管理 | 超时/幂等要求 |
|---|---|---|---|---|---|
| IFD-01 | 公共S3 | HTTPS REST/XML, AWS SigV4 | AccessKey/STS, Policy | 按API逐项矩阵 | 读请求可重试；写请求有operation_id+条件约束 |
| IFD-02 | Git LFS兼容 Batch | HTTPS JSON, LFS Media Type | Git token/OIDC/SSH authorize delegate | Git LFS兼容协议 | Batch本身幂等；upload/verify另行幂等 |
| IFD-03 | LFS Locking | HTTPS JSON | Git仓库写权限 | Git LFS兼容 locking | 锁path唯一、解锁审计 |
| IFD-04 | GitGit Core Hook | localhost unix socket或mTLS HTTPS/JSON | GitGit Core mTLS+签名短token | `/internal/git/v1` | 同Ref old/new+operation_id幂等 |
| IFD-05 | GitGit Core Webhook | mTLS HTTPS/JSON | HMAC/签名、timestamp、nonce | `/internal/git/v1/events` | 至少一次，event_id去重 |
| IFD-06 | Admin API | HTTPS JSON OpenAPI3.1 | OIDC bearer+MFA+RBAC | `/api/v1` | 有限时令牌与重放保护 |
| IFD-07 | Storage Node | gRPC/Protobuf | mTLS service identity | `/storage.v1` | 小块可幂等上传、receipt签名 |
| IFD-08 | Metadata Raft | RPC/WAL/snapshot | 独立mTLS cluster CA | membership兼容 | leader fencing/readIndex |
| IFD-09 | S3 upstream | HTTPS AWS SDK Rust | scoped provider credentials | per-provider | 条件能力探测与限流 |
| IFD-10 | Async event/audit | JSON/OTLP/Webhook/队列 | service auth | schema version | outbox+DLQ+幂等 |

### 1.1 公共接口约束

请求均生成 `x-amz-request-id` 或Git `X-Request-Id`/admin request id；在错误响应提供可追踪的correlation id但不暴露内部凭据。跨LB/proxy必须保留HTTP Host、原始path、query、所有参与签名的header。对外S3响应必须是S3 XML/HTTP headers，不以内部Rust JSON错误代替。

## 2. S3兼容操作最低集合（E0/E1/E2）

| 阶段 | 协议操作 | 关键约束 |
|---|---|---|
| E0 | ListBuckets, Create/Head/DeleteBucket | region和bucket命名边界正确，身份过滤 |
| E0 | Put/Get/Head/DeleteObject | 标准ETag、Content-Type、Range、基本条件头、signed payload |
| E0 | ListObjectsV2 | prefix/delimiter/max-keys/opaque token、稳定排序与权限隔离 |
| E0 | Create/UploadPart/ListParts/Complete/AbortMultipart | 未完成part隐藏、断点重试、按part完整性 |
| E0 | SigV4/Presigned | GET/PUT/HEAD，GET URL不可越权重放到别的Key |
| E1 | CopyObject, Tagging, Metadata, ListObjectVersions | per VersionId透明语义；DeleteMarker处理 |
| E1 | Versioning Status and transitions | Enabled/Suspended状态；空版本/版本消歧及转移测试 |
| E1 | Bucket Policy, STS, SSE-S3/KMS, Lifecycle | 后端不支持时不得悄悄接受写入 |
| E1 | Object Lock, Legal Hold, Retention | 针对对象版本，Governance/Compliance、Bypass和Legal Hold |
| E1 | Checksum（协商子集）、Notifications | 全/分段checksum、事件至少一次 |
| E1 | VirtualHost/PathStyle | Host TLS和SigV4认证路径必须一致 |
| E2 | Batch和Inventory等 | 作为管理作业，而不是谎称AWS所有专属API兼容 |

**单项能力状态定义：** `Supported/Conditional/Unsupported/Unverified`；每一项按 `API + Feature + Mode + Backend + SDK version + Release` 评估。在Federation，如果某操作的后端功能无法由受控网关以同等数据保护语义实现，返回明确不支持或禁止桶路由，而不是返回虚假2xx。

## 3. 版本模型和S3返回行为

版本使能bucket的逻辑`version_id`由目录服务产生，映射到Native manifest generation或上游Physical VersionId。在迁移时对外版本串**保持逻辑不变**，物理串在private映射中更替；如果直接以原provider Host/AK访问，不能保证VersionId不变。临时外部导入对象需扫描、补全目录并封锁旁路写入后才能宣布统一访问有效。 `ETag`不等于SHA-256；Multipart/加密时不假定等于MD5。按对象锁定状态保护特定Version；`DeleteObject`未指定VersionId在Versioning Enabled时可创建DeleteMarker，不意味着删除旧版本。

## 4. S3签名、条件请求及multipart状态

签名层使用原始path+query按AWS规则canonicalize、检查payload hash/stream签名、X-Amz-Date时钟窗、STS session token、signed headers、authz条件。签名错误不得把匿名请求改为公用读权限。支持的`x-amz-content-sha256` 模式通过兼容矩阵明确标注（包括unsigned payload、streamed等条件支持），不支持则拒绝。

Multipart `upload_id`随机且绑定bucket/key/tenant/owner/initiated_at/route_epoch；上传Part校验成功才发布part索引；Complete将parts有序固定并对逻辑对象执行一次版本发布，失败可查询结果。重复`CompleteMultipartUpload`与并发Abort的返回行为需以SDK兼容测试决定，不能假设对任何输入都返回相同成功。清理只处理逾期且无最终manifest引用的分片。

## 5. Git LFS兼容 Batch contract

请求（概念性样例，非完整协议规范）：

```http
POST /team/repo.git/info/lfs/objects/batch HTTP/1.1
Accept: application/vnd.git-lfs+json
Content-Type: application/vnd.git-lfs+json
Authorization: Bearer <scoped-token>

{"operation":"upload","transfers":["basic"],"objects":[{"oid":"<64hex sha256>","size":4294967296}]}
```

响应中每个对象分别返回`actions.upload`（必要时`actions.verify`）或包含per-object error。`download`同理返回`actions.download`或error，不通过Batch回应泄漏其他仓库对象存在。大小与OID检查分流完成后才确认CAS对象存在。OID是SHA-256，拒绝意外算法降级。锁端点包含 `/locks`,`/locks/verify`, `/{id}/unlock`；锁归属关联repo+path并在server-side push核验。Git-lfs标准客户端的HTTP Basic/SSH发现与GitGit Core适配由D-01明确；平台端不假定仓库URL一定满足样例路径。

### 5.1 GitGit Core内部契约（拟定）

| API | 请求字段 | 响应 | 失败规则 |
|---|---|---|---|
| `POST /internal/git/v1/prepare-ref-update` | event_id, repository, actor, old_oid, new_oid, ref, changed-lfs-pointers, Git签名证明 | ticket, expires_at, checked_oid_count, snapshot_digest | 缺少OID/校验/锁/权限即拒绝，GitGit Core不能提交Ref |
| `POST /internal/git/v1/events` | event_id, repository, ref delta, git_transaction_id, timestamp, signature | idempotent ack | 丢失事件补偿/reconciliation |
| `GET /internal/git/v1/repositories/{repo}/refs-snapshot` | snapshot cursor, watermark, signer | signed snapshot of refs and commit pointer reachability | 无有效Git真相时GC fail-closed |
| `POST /internal/git/v1/confirm-ref-update` | ticket, actual old/new, txn id | result | 只确认提交，不能自行篡改GitRef |

注意：`changed-lfs-pointers`由可信GitGit Core通过tree diff解析，**不得仅信任未认证客户端提供的列表**；首次推送、force push、删除Ref、tag与Git fork共享对象在覆盖范围内。如GitGit Core原子更新接口不能实现上述门禁，SRS FR-GIT-005必须列为阻断性集成差异。

## 6. 管理API导航（见 `04_contracts/admin_api_openapi.yaml`）

| 资源 | 路由 | 动作及安全约束 |
|---|---|---|
| Tenant | `/api/v1/tenants` | create/list，组织管理员 |
| Buckets | `/api/v1/buckets` | list/create/manage versioning&retention；敏感变更审批 |
| Git media | `/api/v1/repos/{repo_id}/media` | commit/OID/path检索、授权预览 |
| Backends | `/api/v1/backends` | 注册、证书、capability verify；秘密只显示引用 |
| Migrations | `/api/v1/migrations` | dry-run/create/approve/execute/pause/resume/cutover/rollback |
| Audit | `/api/v1/audits` | 查询、证据验证及导出 |
| Cluster | `/api/v1/cluster/health` | raft、shards、EC、capacity；只读 |
| Jobs | `/api/v1/jobs` | long running状态、checkpoint/结果/错误 |
| Capability | `/api/v1/capabilities` | feature flags per mode/backend/release |

## 7. 安全条件 / Session / 缓存

IAM deny优先+ABAC条件；短期S3 STS身份与Git token分离。登录OIDC会话不自动获管理root权限。撤权以后旧presign能否立即失效取决于gateway签发的credential epoch及失效检查；用网关代理可以撤销，签给上游Provider的预签URL无法完全控制。签发响应不包含长期密钥。对象下载缓存/CDN不得绕过租户ACL、撤销和WORM策略。审计HTTP方法、作用域、决策与错误码，不记录Authorization header和明文query token。

## 8. 通信/容量例外

LB最大body、413处理、慢上传网络超时、client断线、重复Part、LB发生host重写、range跨chunk、Upstream 429/503、clock skew、TLS证书过期、KMS超时均需具备契约测试。Git LFS兼容 20GiB上传不得经过单个JSON/DB字段缓冲；控制面最大批大小、上传最大并发、全部timeout值在详细设计通过性能结果校准。IP allowlist作为条件策略，不取代强身份验证。

## 9. S3/Git错误语义示例

| 场景 | S3/Git对外 | 日志中的根因 |
|---|---|---|
| 未找到Key | XML `NoSuchKey`，HTTP 404 | path, logical version已认证后散列 |
| 错误SigV4 | XML `SignatureDoesNotMatch`等对应错误，HTTP 403 | canonical差异的安全脱敏摘要 |
| 元数据leader不可用 | 503 `ServiceUnavailable` | raft group, quorum status |
| 校验错误 | 400/422按各协议返回，成功对象不能发布 | OID/checksum mismatch |
| 触碰Compliance retention | 403 `AccessDenied` | lock_version, policy_decision |
| Git push未存在OID | Git pre-receive rejection | repo/ref/missing count, no secret |

## 10. 未完成验证

需在CI用AWS CLI、boto3、JS SDK v3、Go/Rust SDK以及git-lfs官方客户端执行实测。S3有大量交叉语义（checksum、versioning、Object Lock、ACL、conditional requests、virtual-host）、不能仅验证HTTP状态码，需要校对响应header/body、对象字节与状态变迁。
