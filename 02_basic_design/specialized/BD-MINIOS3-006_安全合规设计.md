# BD-MINIOS3-006 — 安全、身份、加密、WORM与审计基本设计

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 对应 FR-SEC-001~014、FR-S3-006/007/010/012/014、NFR-SEC-001~006。安全P0问题不得延期到UI上线后修补。

## 1. 可信边界/STRIDE初始威胁模型

| 信任边界 | 主要威胁 | 控制 | 证明方式 |
|---|---|---|---|
| Public S3 → Gateway | SigV4欺骗、重放、请求走私、Host混淆 | TLS、精确canonical、时间窗、request limits、Host正则化前签名检查 | fuzz+协议回归+PENTEST |
| GitGit Core → LFS/Hook | 假Ref事件、伪OID或绕过锁 | mTLS、签名timestamp/nonce、可信tree解析、ref-update事务票据 | Git force-push/lock bypass测试 |
| Tenant A → Tenant B | 枚举CAS OID侧信道、缓存穿透、S3键串用 | 默认拒绝、租户隔离AuthZ、限额、cache带权限epoch | cross-tenant渗透 |
| Gateway → Backend | 凭据泄露、TLS中间人、provider接管 | 短期/最小权限凭据、secretRef、mTLS/CA验证、allowlist | secret扫描、故障注入 |
| Storage Node ↔ Meta | 伪造receipt/旧leader写入 | 签名receipt、服务身份、routeEpoch、quorum | 网络分区故障测试 |
| Worker → 原始媒体 | 恶意转码利用/SSRF/host escape | 只读输入、受限sandbox、无特权、无默认egress、限时限量 | 不可信文件攻击集 |
| 管理员 → WORM | 强行缩短retain/clock rollback | Compliance版本拒绝，可信时间单调下界，四眼审批和外部审计 | WORM/时钟攻击演练 |
| 审计外发 | 删除/重放/伪造事件 | Raft outbox、hash-chain、外部独立WORM、断点重发 | tamper/delay/drop模拟 |

## 2. 身份授权模型

- 管理面支持OIDC登录与MFA（IdP可提供验证），session使用短期Cookie或Bearer Token并设置CSRF/CORS策略；用户登出/撤销时更新policy epoch。
- S3 SigV4的AccessKey映射到服务Principal；STS短期令牌包括tenant、role、expire、session nonce与scope。不能仅凭用户属于tenant就允许读取所有Git资产。
- Git访问经repo policy和读/写权限，LFS Batch对OID的访问必须同时存在repo授权链接和内容存在；不存在权限时统一适当错误避免存在性侧信道。
- IAM deny precedence：显式Deny > Allow，未匹配默认Deny；WORM/legalHold是额外强制条件不能被普通Allow覆盖；ABAC条件覆盖时间/IP/tag/KMS key/region及MFA等。

### 2.1 角色最小矩阵（样例，实际权限可按政策细化）

| 操作 | StorageAdmin | TenantAdmin | RepoMaintainer | Developer | Auditor |
|---|---:|---:|---:|---:|---:|
| 添加backend | ✓（双人审批） | — | — | — | Read |
| 访问/导出tenant原件 | —（默认） | 由策略授权 | 仓库授权 | 仓库授权 | 依审计策略 |
| 修改Bucket Policy | 审批或策略 | ✓（有作用域） | — | — | Read |
| Git LFS兼容上传 | — | 授权后 | ✓ | ✓ | — |
| Git LFS兼容强制解锁 | — | 策略允许 | 有理由+审批 | 自有锁 | Read |
| 迁移切换 | ✓（审批） | 申请 | — | — | Read |
| 读取安全审计 | 限状态 | 仅作用域 | — | — | ✓ |
| 缩短Compliance retention | **任何角色均不允许** | 不允许 | 不允许 | 不允许 | 不允许 |

`StorageAdmin`不拥有全租户明文数据权限；危险操作需独立合规审计员或第二审批人按策略确认。

## 3. 密码学与密钥生命周期

| 对象 | 方式 | 风险控制 |
|---|---|---|
| 客户端传输 | TLS1.2+ / 建议TLS1.3 | 必须校验Host/证书；必要时mTLS |
| 服务间 | SPIFFE/SVID或组织CA签发mTLS | 轮换、撤销、证书过期报警 |
| 原件静态 | 信封加密 + 每对象/块DEK | AEAD nonce唯一；chunk digest与认证标签 |
| DEK包裹 | KMS/HSM envelope encrypt | 存KeyRef/包裹密钥，不保存明文主钥 |
| S3 SSE-KMS | 显式兼容请求/响应语义 | 物理目标与网关“谁加密”责任清单 |
| 临时下载token | 短有效期+scope+签名+revocation epoch | 不将Bearer透传给不可信后端 |
| Git Hook证书 | 短期mTLS/签名密钥 | replay nonce cache持久化/定期轮换 |

加密与对象checksum均为端到端校验链条的一部分；加密前原文SHA与密文分片完整性分别记录，KMS不可用不自动退回SSE-S3未加密方案。精确算法/nonce/state replay在DD进行密码学评审。

## 4. WORM/Retention/LegalHold实效界限

1. 对象锁是**版本级**，新版本和DeleteMarker在协议允许情况下可以存在，旧受保护版本不可删。Versioning与Object Lock的启用约束参照AWS语义逐例验证。
2. **Governance**：有专门权限且请求显式bypass header时可以在规范允许下绕过Governance；Legal Hold独立阻止版本删除。**Compliance**：普通应用管理员、策略管理员无法缩短保留或提前删除。
3. 受保护物理分片可在等价内容hash被确认后迁移/修复到符合保护要求的目标，但删除旧副本必须先满足不可违反的保留制度；不能以“迁移”开后门。
4. Native软件在物理root可直接破坏磁盘的威胁模型下无法单凭应用API给出相当AWS物理隔离的保证；若需要监管合规应引入外部WORM/硬件信任根/第三方鉴证并收敛威胁边界。
5. 对重要WORM桶仅允许 `verified_worm_backend==true`，验收证明包括Retention不可缩短、LegalHold、time rollback、copy/restore、delete-marker行为、迁移恢复。

## 5. 时间与防回滚

可信时间由经身份认证的服务/运行基础设施提供；WORM decision维护已持久化的`last_trusted_time_lower_bound`，遇可信时间回退进入保护模式；领导者变更必须从Raft恢复时间下界。若可信时间无法验证，拒绝需要强保留保证的新敏感操作；既存锁定版本不因时间源异常提前到期。不能声称系统能对root直接调盘/断电的所有攻击完全防护。

## 6. 审计证据（谁/何时/何处/哪个版本/结果）

每次写操作/策略/KMS/Git Ref/迁移/删除/越权拒绝产生：`event_id,trace_id,tenant_id,actor_principal,action,object_ref,version_ref,request_ip_hash,policy_epoch,decision,raft_index,prev_audit_hash,created_at,signature_ref,source_service`。关键事件写入Raft outbox，与业务变更同一事务；异步外送到独立WORM安全域，外送水位可查询，堆积触发Sev升级。读取审计可采样但对象访问是否必须全量记录由合规要求签署；不能以记录量过高而静默丢弃法律必需事件。

## 7. 数据驻留与删除合规

Tenant/桶定义`allowed_regions/allowed_backends/retention_min/legal_hold`；路由和迁移前执行策略决策，目标位置未知则Blocked。租户删除先进入soft-disable，清点Git引文、历史Version、审计、WORM和法律保留；只对可依法删除部分实施密钥清除或物理回收。不能将“擦除一个密钥”在未评审共享加密/副本情况下等同实际完整删除。

## 8. 软件供应链及部署安全

Rust lint、clippy、fuzz、miri适用性、SAST/SCA、依赖license scan、SBOM、artifact signing、双人发布审批、最小容器cap、read-only rootfs、seccomp、NetworkPolicy、secrets in KMS refs。受商业许可证限制的媒体处理组件作为插件不自动纳入内核或商业交付。

## 9. 安全验收用例（必须失败的请求）

- 跨租户通过已知OID尝试Batch下载、GET对象、查询Git映射；不能泄露真实存在性。
- 恶意请求重排signed headers/改变Host/编码路径、过期签名、伪造x-amz-date；拒绝且有请求ID。
- Object Lock开启期间尝试admin提前删除、绕过Governance缺失明确header、对Compliance延迟时间源，必须按规则拒绝。
- 已发布presign在身份撤销后通过网关请求；测试撤权收敛，并记录不可即时撤销的外部direct presign已禁用。
- 伪造GitWebhook/重放event_id、跳过客户端锁/force push；server-side门禁拒绝。
- 恶意视频/图片/3D解析器反向Shell、SSRF、OOM攻击；worker sandbox无法接触tenant其他资产和host secrets。
- KMS拒绝解密时不得写明文fallback；审计导出目标断开不得静默丢弃本地outbox。
