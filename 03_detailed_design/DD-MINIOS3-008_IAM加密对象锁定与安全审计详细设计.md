# DD-MINIOS3-008 — IAM加密对象锁定与安全审计详细设计

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：SSO、STS、KMS、WORM、审计、凭据


## 1. 权限模型与安全边界

主体`User, ServiceAccount, GitGit Core, Worker, Auditor`；资源`Tenant, Project, Bucket, Key, Version, Repo, Job`；动词`Get,Put,List,DeleteVersion,Hold,Approve,Cutover,RotateKey,AuditExport`。Decision顺序：认证→tenant scope→显式deny→资源与条件Policy→Object Lock/LegalHold→后台岗位分离→Quota/Capacity→Permit。任何Unauthorized不得因为CAS OID存在、共享缓存命中或Provider公桶变成Allow。

## 2. Role Matrix（示例基线）

| 动作 | TenantAdmin | RepoWriter | Auditor | Ops | SecurityApprover |
|---|---|---|---|---|---|
| S3本项目PUT/GET | permit by policy | permit scope | deny unless granted | deny unless break-glass | deny |
| LFS Batch/Lock | repo role constrained | permit scoped | readonly metadata by grant | deny | deny |
| Migration Dry-run | permit | deny | readonly evidence | permit | permit |
| Migration Cutover | request only | deny | audit only | execute after approval | approve only |
| Retention shorten | Governance按特权+审计；Compliance拒绝 | deny | deny | deny | 不能绕过Compliance |
| Audit log purge | deny | deny | deny | deny | deny（外部留存规则） |

强制审批申请人与最终批准人为不同身份，临时BreakGlass也不得突破Compliance物理/法定边界。RBAC用于管理员角色，ABAC用于tenant/region/Key/源IP/调用时段等条件，资源路径逐层鉴权。

## 3. 认证和凭据

S3 Key/STS专用凭据使用不可逆hash索引与SecretRef；AccessKey撤销提升`policy_epoch`，全部网关短缓存需重查epoch，不得等TTL结束自动失效。OIDC id_token必须验证issuer/audience/nonce/exp；Git身份由受信GitGit Core受限凭据委托。所有管理敏感操作二次认证/MFA，日志只记CredentialId散列。

## 4. 密钥加密

TLS 1.2+，内部mTLS；静态数据AEAD信封加密：per-object/per-encryption-generation DEK由受控KMS key加密，存储仅保存wrapped DEK引用。AES-GCM nonce唯一性需UT性质测试；上传重试若加密内容不同必须换新的generation/key nonce组合。KMS错误不回退到明文，不允许全量对象解密落日志/临时目录；rotate使用reenvelope或重写分片方式并全链路校验。

## 5. Object Lock状态规则

- `Mode=GOVERNANCE`：仅具合法Bypass权限、对应请求标志且审计可证明时允许依法解除；普通用户拒绝。
- `Mode=COMPLIANCE`：保留期内不得缩短/删除；即使软件管理员也不得通过普通API绕过。
- LegalHold独立于Retention，必须在明确授权下切换；删除某个特定已锁Version=拒绝，Versioning Enabled的无VersionId DELETE依AWS允许语义可创建DeleteMarker。
- 迁移只能到能力行为已验真的后端；Retention在Source/Target的有效截止时间不得缩短；没有可信时间和底层保护的实现不得对外自称法规WORM认证。

## 6. 时间、审计和证据

Retention计算依赖可信外部时间与防回滚观测，时间不可用时拒绝提前解锁。业务提交与AuditOutbox同Raft事务写入`event_id, tenant, actor, action, targetLogicalVersion, repo_commit_ref, decision, policy_revision, result, trace, occurred_at, prev_hash`。Outbox对外独立不可变sink至少一次投递且去重、watermark+dead-letter；审计长期保留草案≥365天须合规负责人批准。不可存SecretKey、query signature及DEK。

## 7. STRIDE与发布门禁

Spoofing：伪造GitWebhook/STS；Tampering：manifest位改写/Retention后改；Repudiation：操作员删审计；Info Disclosure：猜OID/缓存跨租户；DoS：MPU海量未完成、429雪崩；Elevation：角色横向越权/强制解锁。各类均有UT policy、IT接口、ST攻击/恢复测试。供软件供应链检查SBOM/签名/许可证，未接受的Critical CVE=0；不直接复用MinIO企业版专有实现与版权内容。
