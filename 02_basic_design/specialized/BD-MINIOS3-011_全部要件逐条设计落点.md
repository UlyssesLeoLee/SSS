# BD-MINIOS3-011 — 158项功能要件逐条设计落点

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 这是BD-001~010的逐项**实现方法索引**，不替换SRS中的要求/验收内容。设计状态：Draft；所有接口与代码尚未测试。

每条行号可与`MinIOS3_GitGit_IPA_Traceability_v2.0.xlsx`对应，测试编号为后续DD/UT/IT/ST的规划入口。

## CORE 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-CORE-001 | 同一Rust二进制可部署到VM/K8s，内网IdP/KMS可用时无需公网上控制面；公网Fed后端除外。 | BD-001/BD-004/BD-008/BD-010 | E0 |
| FR-CORE-002 | 网关后置Backend trait；Native与Federation桶在同一Catalog统一定位，Mixed仅指不同桶并存，不是双主写入。 | BD-001/BD-004/BD-008/BD-010 | E1 |
| FR-CORE-003 | Tenant→Project→Bucket/Repo的强类型权限域；同名资源必须按tenant和bucket_id鉴权。 | BD-001/BD-004/BD-008/BD-010 | E0 |
| FR-CORE-004 | QuotaReservation在元数据prepare/commit/cancel原子结算；实时与核账报表分离。 | BD-001/BD-004/BD-008/BD-010 | E1 |
| FR-CORE-005 | 公开Bucket/Key稳定；route epoch和placement mapping隔离供应商地址/物理VersionId。 | BD-001/BD-004/BD-008/BD-010 | E0 |
| FR-CORE-006 | 配置revision+差异预览+四眼审批，回滚只对仍相容的配置有效。 | BD-001/BD-004/BD-008/BD-010 | E1 |
| FR-CORE-007 | Bucket路由校验region allowlist与provider数据驻留证据，异地复制前执行同一Policy。 | BD-001/BD-004/BD-008/BD-010 | E1 |
| FR-CORE-008 | 发布模式/API/Provider的机器可读能力表，Unverified不能以Supported显示。 | BD-001/BD-004/BD-008/BD-010 | E1 |

## S3 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-S3-001 | Bucket CRUD、global name uniqueness、region解析和删除空桶前逐版本/未完成上传核验。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-002 | PUT/GET/HEAD/DELETE使用Raft目录与后端流式I/O；错误为S3 XML；确认顺序见BD-005。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-003 | ListV2有序Key迭代+opaque token；跨range LIST证明未完成前保持单Bucket单group。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-004 | Part以upload_id+part_number+etag记账；Complete先验证各part后原子发布版本，Abort仅删无引用Part。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-005 | Public LogicalVersionId与physical VersionId分离，Enabled/Suspended/DeleteMarker分别建状态。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-006 | 原始Host/path/query和signed headers参与SigV4鉴权；坏签名、时钟与STS过期拒绝。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-007 | 短期网关签名链接绑定Bucket/Key/method/policy_epoch，撤权不保证绕过网关的Provider签名。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-008 | Range/If-Match/If-None-Match按已签SDK矩阵实现；无需全对象入内存。 | BD-003/BD-004/BD-005 | E0 |
| FR-S3-009 | Tag/metadata保存在ObjectVersion中；CopyObject在目标backend capability校验后执行。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-010 | IAM deny优先+Bucket Policy+有限ACL兼容，在不等价ACL后端拒绝危险请求。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-011 | 生命周期worker先校验锁/法律保留/Git CAS引用，再标记可清理版本。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-012 | Version-level Object Lock，Governance专权+header、Compliance不能缩短、LegalHold独立。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-013 | 每次目录commit写Raft outbox事件，至少一次投递且event_id幂等。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-014 | SSE-S3/KMS能力检查、信封加密与keyRef；KMS不可用不明文回退。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-015 | SHA-256验证对象原始字节；CRC32C验证分片；ETag不可当完整性唯一依据。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-016 | E2长批任务走检查点状态机，写入动作逐版本授权与审计。 | BD-003/BD-004/BD-005 | E2 |
| FR-S3-017 | CI产出逐API×SDK×Mode×Provider×版本矩阵与证据链接。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-018 | 支持path-style和host-style，LB保持原始host/path供签名，证书/DNS按bucket匹配。 | BD-003/BD-004/BD-005 | E1 |
| FR-S3-019 | Raft业务提交带operation_id/conditional头校验，避免超时重放生成不可解释版本。 | BD-003/BD-004/BD-005 | E1 |

## GIT 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-GIT-001 | 实现LFS Batch upload/download/verify actions，仓库权限在返回每对象action之前判定。 | BD-003/BD-004/BD-006 | E0 |
| FR-GIT-002 | Rust Locking API以repo+path为范围；GitGit Core pre-receive再次核验，不信客户端自检。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-003 | Git commit/tree/ref由GitGit Core管理，MinIOS3只存CAS和索引，不改Git对象格式。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-004 | SHA-256 OID+size匹配后写不可变CAS，按tenant级去重并做repo授权链接。 | BD-003/BD-004/BD-006 | E0 |
| FR-GIT-005 | GitGit Core可信解析changed LFS pointer并在原子Ref CAS之前申请准备票据；缺失即拒绝。 | BD-003/BD-004/BD-006 | E0 |
| FR-GIT-006 | Repo/Commit/Path/OID/LogicalVersion/Placement索引可审计，GitGit Core为Ref真相。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-007 | 切分支时根据Git Commit对应指针定位OID，不生成新S3或LFS版本。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-008 | Git Token/JWT经受信GitGit Core或IdP验证且scope到repo，禁止仅凭OID存在授权。 | BD-003/BD-004/BD-006 | E0 |
| FR-GIT-009 | 有签名的Ref events按event_id去重，定期完整reconcile修复事件丢失。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-010 | GC先获取refs真实签名快照/保持水位，综合Snapshot/WORM/LegalHold/TTL二阶段复核。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-011 | E2按需获取对象与断线重试，不改变Git仓库/历史，离线缓存指纹验证。 | BD-003/BD-004/BD-006 | E2 |
| FR-GIT-012 | Repo上传前预留quota、请求与带宽限额；超额不清除已有历史。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-013 | LFS数据准备和Git Ref完成是两个事务；补偿检查orphan/missing链路。 | BD-003/BD-004/BD-006 | E1 |
| FR-GIT-014 | E2导入旧LFS remote时保留Oid，逐SHA验证并在GitRefs切换前追平。 | BD-003/BD-004/BD-006 | E2 |

## MEDIA 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-MEDIA-001 | 任意二进制按原字节保存，媒体类型只是索引/预览选择依据。 | BD-002/BD-008/BD-006 | E0 |
| FR-MEDIA-002 | 媒体metadata按源OID和commit引用关系保存，大小时长尺寸附来源及可信度。 | BD-002/BD-008/BD-006 | E0 |
| FR-MEDIA-003 | 存储对象与预览artifact目录分离，转换不覆盖原件且sha可验证。 | BD-002/BD-008/BD-006 | E0 |
| FR-MEDIA-004 | E1预览经异步worker与缓存，失败返回可见状态不阻止下载。 | BD-002/BD-008/BD-006 | E1 |
| FR-MEDIA-005 | E2差异插件按格式能力发现，原图/源文件始终留存。 | BD-002/BD-008/BD-006 | E2 |
| FR-MEDIA-006 | 受tenant授权的派生搜索索引，返回index watermark防止误认实时完整性。 | BD-002/BD-008/BD-006 | E1 |
| FR-MEDIA-007 | worker沙盒限CPU/RAM/FD/时间/网络，只授最小文件读取权。 | BD-002/BD-008/BD-006 | E1 |
| FR-MEDIA-008 | 派生物记录source OID、转换器版本、参数、生成时间与checksum。 | BD-002/BD-008/BD-006 | E1 |
| FR-MEDIA-009 | Range+内容分发缓存受权限与撤权epoch约束，跨域缓存不泄漏。 | BD-002/BD-008/BD-006 | E1 |
| FR-MEDIA-010 | E2版权授权/创作者/许可有效期及发布审批实现独立policy。 | BD-002/BD-008/BD-006 | E2 |

## XCHG 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-XCHG-001 | AWS SDK Rust实现provider适配并维持header/版本/校验能力证据。 | BD-007/BD-003 | E0 |
| FR-XCHG-002 | E2非S3 adapter经trait和隔离sidecar扩展，不承诺语义自动等价。 | BD-007/BD-003 | E2 |
| FR-XCHG-003 | 公开S3 Host、Bucket、LogicalVersion保持不变，底层location私有。 | BD-007/BD-003 | E0 |
| FR-XCHG-004 | 登记Versioning/WORM/Checksum/SSE/Region等能力并运行探测，Unknown→FailClosed。 | BD-007/BD-003 | E0 |
| FR-XCHG-005 | Policy按tenancy/size/region/cost/latency分类，记录route理由与版本。 | BD-007/BD-003 | E1 |
| FR-XCHG-006 | 迁移Job持久checkpoint并包含historical Version、delete markers、tags/retention。 | BD-007/BD-003 | E0 |
| FR-XCHG-007 | DryRun输出源目标差异、成本、版本数、不可迁移阻断项。 | BD-007/BD-003 | E0 |
| FR-XCHG-008 | 源主写/目标影子复制+write fence+final delta校验+route epoch CAS；回滚需反向追平。 | BD-007/BD-003 | E0 |
| FR-XCHG-009 | 两端原始SHA-256、长度、Version次序、tags、metadata/retention逐项证据；ETag不可代替。 | BD-007/BD-003 | E1 |
| FR-XCHG-010 | 异步单主复制有事件顺序、retry/DLQ、持续滞后watermark指标。 | BD-007/BD-003 | E1 |
| FR-XCHG-011 | 拒绝未定义双主，物理写入携带route epoch并由元数据拒绝旧epoch。 | BD-007/BD-003 | E1 |
| FR-XCHG-012 | 读取切副本必须通过lag/RPO与权限/WORM确认，不能谎称没有异步窗口。 | BD-007/BD-003 | E1 |
| FR-XCHG-013 | 开放导出逻辑版本清单与physical映射、证据摘要及恢复脚本说明。 | BD-007/BD-003 | E1 |
| FR-XCHG-014 | E2存储/网络API/迁移出站成本按provider和tenant聚合对账。 | BD-007/BD-003 | E2 |
| FR-XCHG-015 | 供应商429/503/限额以隔离后端队列和指数退避执行，不拖垮其它Bucket。 | BD-007/BD-003 | E1 |

## NATIVE 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-NATIVE-001 | Raft meta组+存储节点分离，扩容采用新placement generation重平衡。 | BD-005/BD-010 | E1 |
| FR-NATIVE-002 | B1以4+2 EC为候选，数据receipts全部durable后Raft commit，不以四片能解码作为ACK。 | BD-005/BD-010 | E1 |
| FR-NATIVE-003 | 碎片按AZ/Node/Disk不同故障域放置，六节点布局失一AZ后读可恢复写安全限制。 | BD-005/BD-010 | E1 |
| FR-NATIVE-004 | Scrub发现片损坏后先新位置重建并校验，manifest切换后才回收旧片。 | BD-005/BD-010 | E1 |
| FR-NATIVE-005 | write ACK后GET/HEAD/List看Raft线性化目录；跨range LIST必须证明全局一致性。 | BD-005/BD-010 | E1 |
| FR-NATIVE-006 | 控制/数据进程故障互隔离，metadata quorum失效时不放行不一致写。 | BD-005/BD-010 | E1 |
| FR-NATIVE-007 | 磁盘水位/保留空间和限流均显式展示，无法合规放置时拒绝。 | BD-005/BD-010 | E1 |
| FR-NATIVE-008 | CRC32C片校验+原文SHA-256、manifest摘要、持续Bit-rot scrub与证据记录。 | BD-005/BD-010 | E1 |
| FR-NATIVE-009 | Chaos用断节点/AZ/网络/时钟/盘满，验证故障点与重放恢复。 | BD-005/BD-010 | E1 |

## SEC 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-SEC-001 | OIDC接入企业IdP、MFA会话保护和撤销policy epoch。 | BD-006 | E1 |
| FR-SEC-002 | IAM/RBAC/ABAC统一PolicyEngine，显式Deny+Tenant scope全路径执行。 | BD-006 | E1 |
| FR-SEC-003 | STS短期scope token与长期AK分离；缓存有失效/撤权栅栏。 | BD-006 | E1 |
| FR-SEC-004 | 外部TLS、内部mTLS、凭据脱敏、证书轮换告警与最小端口。 | BD-006 | E1 |
| FR-SEC-005 | AEAD信封加密，每对象块独立DEK/keyRef，KMS拒绝不降级明文。 | BD-006 | E1 |
| FR-SEC-006 | Raft outbox统一记录写/删/策略/KMS/Git行为，按trace关联审计。 | BD-006 | E1 |
| FR-SEC-007 | 外部独立WORM sink与hash-chain、投递水位监控、断链再送。 | BD-006 | E1 |
| FR-SEC-008 | 可选AV/DLP worker隔离媒体，原件字节不变并可留取隔离证据。 | BD-006 | E1 |
| FR-SEC-009 | MFA、敏感操作双人批准、role separation、审计和撤销。 | BD-006 | E1 |
| FR-SEC-010 | SBOM/SCA/license scan、发行签名、最小权限容器、密钥泄漏测试。 | BD-006 | E1 |
| FR-SEC-011 | tenant出口/删除须先检查法律保留、历史Version、Git引用/region约束。 | BD-006 | E1 |
| FR-SEC-012 | secretRef访问而非明文配置，支持轮换与租户隔离。 | BD-006 | E1 |
| FR-SEC-013 | 版本保留+不可变灾备副本+异常批量删除告警。 | BD-006 | E1 |
| FR-SEC-014 | 声称Compliance必须外部法律/设施评估；UI明确Uncertified。 | BD-006 | E1 |

## OPS 功能组

| 要件ID | 设计方案（最终目标态） | 设计分册 | 阶段 |
|---|---|---|---|
| FR-OPS-001 | SCR-001~015由Rust Admin API驱动，管理操作与Gateway数据面相分离。 | BD-008/BD-010 | E0 |
| FR-OPS-002 | 可视化Ref→Commit→Path→OID→Version→Placement并标记索引水位。 | BD-008/BD-010 | E1 |
| FR-OPS-003 | 迁移State、blockers、checkpoint、approval、cutover/rollback一屏追踪。 | BD-008/BD-010 | E1 |
| FR-OPS-004 | Bucket/Key/version/retention/preview分页查看，保护标记清晰。 | BD-008/BD-010 | E1 |
| FR-OPS-005 | 语言资源zh-CN/ja-JP/en-US，错误代码稳定与时间单位本地化。 | BD-008/BD-010 | E1 |
| FR-OPS-006 | 界面少弹层、危险动作两步确认与读屏/键盘可达性。 | BD-008/BD-010 | E2 |
| FR-OPS-007 | Rust minios3ctl统一Admin API；Config revision与Dry-run，不超权限。 | BD-008/BD-010 | E1 |
| FR-OPS-008 | OpenAPI3.1管理契约和S3 SDK示例，发版附真实兼容矩阵。 | BD-008/BD-010 | E2 |
| FR-OPS-009 | 审计按tenant/request_id/commit/oid查询并验证外部sink回执。 | BD-008/BD-010 | E1 |
| FR-OPS-010 | Raft/EC/RPO/KMS/queue metrics及Sev1 5min告警和runbook。 | BD-008/BD-010 | E0 |
| FR-OPS-011 | N/N-1灰度滚动升级，元数据版本与不可逆schema变更前置备份。 | BD-008/BD-010 | E1 |
| FR-OPS-012 | Raft WAL/snapshot、blob manifests、key refs和Git refs一体DR。 | BD-008/BD-010 | E1 |
| FR-OPS-013 | 错误统一分层分类与request_id，避免回显内部敏感堆栈。 | BD-008/BD-010 | E1 |
| FR-OPS-014 | RUN-001..012列明操作步骤、权限、禁做事项、升级链。 | BD-008/BD-010 | E1 |
| FR-OPS-015 | SLO按读写API/后端/可用率分拆测量并公开故障区间。 | BD-008/BD-010 | E1 |

## 非功能要件（32项）逐条落点

| NFR ID | 指标/设计措施 | 设计分册 | 验收方式 |
|---|---|---|---|
| NFR-AVA-001 | 生产可用率：月度 S3 API 可用率≥99.95%（草案）；具体测试条件见原SRS及BD-009。 | BD-005/BD-007/BD-010 | MEASURE |
| NFR-AVA-002 | 确认写入持久性：已返回2xx的对象不得丢失，损坏可自动检出；具体测试条件见原SRS及BD-009。 | BD-005/BD-007/BD-010 | CHAOS |
| NFR-AVA-003 | 站点恢复：生产RTO≤60分钟；异步跨站RPO≤15分钟（草案）；具体测试条件见原SRS及BD-009。 | BD-005/BD-007/BD-010 | DRILL |
| NFR-AVA-004 | 同站数据一致性：ACK后GET/HEAD/LIST强一致；跨站异步复制单独声明；具体测试条件见原SRS及BD-009。 | BD-005/BD-007/BD-010 | TEST |
| NFR-AVA-005 | WORM恢复安全：保留版本在保留期限内不得通过应用API提前删除；具体测试条件见原SRS及BD-009。 | BD-005/BD-007/BD-010 | CHAOS |
| NFR-AVA-006 | 恢复演练周期：季度DR演练；每月备份恢复抽样验证；具体测试条件见原SRS及BD-009。 | BD-005/BD-007/BD-010 | INSPECT |
| NFR-PER-001 | S3对象延迟：GET p95≤150ms；PUT p95≤300ms（不含公网）；具体测试条件见原SRS及BD-009。 | BD-002/BD-005/BD-009 | BENCH |
| NFR-PER-002 | 数据吞吐：集群聚合读取吞吐≥1GiB/s，具体由硬件验收确认；具体测试条件见原SRS及BD-009。 | BD-002/BD-005/BD-009 | BENCH |
| NFR-PER-003 | 存储规模：≥1亿对象、≥1PiB逻辑命名空间可寻址（压力验证可分层）；具体测试条件见原SRS及BD-009。 | BD-002/BD-005/BD-009 | BENCH |
| NFR-PER-004 | 弹性扩容：扩容期间原API保持可用，数据完整，性能下降范围形成报告；具体测试条件见原SRS及BD-009。 | BD-002/BD-005/BD-009 | DRILL |
| NFR-PER-005 | Git媒体规模：可上传/下载/恢复/索引且不出现O(size)内存增长；具体测试条件见原SRS及BD-009。 | BD-002/BD-005/BD-009 | BENCH |
| NFR-PER-006 | 并发公平性：限流生效，任一租户无法持续耗尽全局带宽/IOPS；具体测试条件见原SRS及BD-009。 | BD-002/BD-005/BD-009 | BENCH |
| NFR-OPS-001 | 指标覆盖：请求、错误、延迟、容量、校验、排队、滞后均有指标；具体测试条件见原SRS及BD-009。 | BD-008/BD-009/BD-010 | INSPECT |
| NFR-OPS-002 | 部署和回滚：声明式安装、滚动升级、版本回退操作步骤经演练通过；具体测试条件见原SRS及BD-009。 | BD-008/BD-009/BD-010 | DRILL |
| NFR-OPS-003 | 数据修复透明：有操作日志、校验报告、版本证据，不允许静默覆盖；具体测试条件见原SRS及BD-009。 | BD-008/BD-009/BD-010 | TEST |
| NFR-OPS-004 | 代码质量：自动化测试覆盖率目标≥90%；关键路径100%分支用例设计；具体测试条件见原SRS及BD-009。 | BD-008/BD-009/BD-010 | CI |
| NFR-OPS-005 | 版本升级兼容：API、数据格式与元数据迁移可回退或前置备份校验；具体测试条件见原SRS及BD-009。 | BD-008/BD-009/BD-010 | DRILL |
| NFR-OPS-006 | 故障响应：Sev1自动告警≤5min，操作手册有明确责任人和升级路径；具体测试条件见原SRS及BD-009。 | BD-008/BD-009/BD-010 | TEST |
| NFR-MIG-001 | 迁移数据准确度：对象SHA-256及字节长度 100%一致；元数据差异全部列明；具体测试条件见原SRS及BD-009。 | BD-003/BD-007/BD-009 | BENCH |
| NFR-MIG-002 | 迁移中断恢复：重启后接续、不出现漏项；无法迁移对象附异常报告；具体测试条件见原SRS及BD-009。 | BD-003/BD-007/BD-009 | CHAOS |
| NFR-MIG-003 | 迁移回滚：失败切换可逆且不会覆盖源站已确认写入；具体测试条件见原SRS及BD-009。 | BD-003/BD-007/BD-009 | DRILL |
| NFR-MIG-004 | 外部兼容性：宣称兼容功能通过协议回归测试≥99%；0个阻断级差异；具体测试条件见原SRS及BD-009。 | BD-003/BD-007/BD-009 | TEST |
| NFR-SEC-001 | 权限拒绝：敏感资源默认拒绝；跨租户访问成功数=0；具体测试条件见原SRS及BD-009。 | BD-006/BD-009 | PENTEST |
| NFR-SEC-002 | 加密：TLS1.2+；静态强加密并依赖受控KMS；禁用明文后退；具体测试条件见原SRS及BD-009。 | BD-006/BD-009 | INSPECT |
| NFR-SEC-003 | 凭据处理：不能输出密钥明文；密钥轮换和吊销符合审计要求；具体测试条件见原SRS及BD-009。 | BD-006/BD-009 | PENTEST |
| NFR-SEC-004 | 漏洞要求：0个已知未接受的Critical漏洞；高危经审批例外；具体测试条件见原SRS及BD-009。 | BD-006/BD-009 | SCAN |
| NFR-SEC-005 | 审计保留：审计事件可关联且在外部不可变存储受控保留≥365天（草案）；具体测试条件见原SRS及BD-009。 | BD-006/BD-009 | INSPECT |
| NFR-SEC-006 | 供应链验证：SBOM齐全、构建物签名、授权许可证扫描完成；具体测试条件见原SRS及BD-009。 | BD-006/BD-009 | SCAN |
| NFR-ENV-001 | 支持环境：支持矩阵中的环境可重复部署且升级回滚通过；具体测试条件见原SRS及BD-009。 | BD-002/BD-010 | TEST |
| NFR-ENV-002 | 资源效率：资源预算以性能基线测试建立，不以未验证的绝对值承诺；具体测试条件见原SRS及BD-009。 | BD-002/BD-010 | BENCH |
| NFR-ENV-003 | 离线可维护性：无公网时安装镜像、文档、许可证/密钥方案可正常运作；具体测试条件见原SRS及BD-009。 | BD-002/BD-010 | DRILL |
| NFR-ENV-004 | 地域节能策略：可呈现资源耗用及预估迁移成本，不因优化误删受保护版本；具体测试条件见原SRS及BD-009。 | BD-002/BD-010 | INSPECT |

## 设计可追溯性约束

- “覆盖”仅表示存在明确设计文件与测试规划标识，不等于完成、测试通过或获得认证。
- 任何要件若因真实Git接口、WORM合规、分布式LIST等风险无法实现，提交CR并保留原ID，不能直接删去原要求。
- 每次BD/DD/代码变动均须更新FR/NFR追踪、测试设计和发行兼容矩阵。


## v2.0 GitGit 原生与媒体扩展要件映射（增量已纳入正式设计）

| ID | 要件 | 基本设计 | 详细设计 | UT / IT / ST 关联 |
|---|---|---|---|---|
| FR-GG-001 | GitGit仓库权威 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-002 | 不可变提交图 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-003 | 原子Ref CAS | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-004 | 仓库快照 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-005 | 两阶段提交门禁 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-006 | 幂等确认与Outbox | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-007 | 树级变更与重命名 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-008 | 分支差异查询 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-009 | 原生资产锁 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-010 | 离线工作区日志 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-011 | 稀疏工作区与懒加载 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-012 | 三方合并引擎 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-013 | 冲突决议审计 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-014 | 媒体评审流 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-015 | 依赖变更影响 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-016 | 历史可达性水位 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-GG-017 | 组织级授权 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-018 | 受保护提交策略 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-019 | 标准Git迁入隔离 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 1 |
| FR-GG-020 | 提交与资产恢复 | BD-MINIOS3-012 | DD-MINIOS3-014 | 1 / 1 / 2 |
| FR-MV-001 | 稳定资产标识 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 1 |
| FR-MV-002 | 媒体版本记录 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-003 | 内容清单 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 1 |
| FR-MV-004 | 内容定义分块 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 2 |
| FR-MV-005 | 强摘要验证 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 1 |
| FR-MV-006 | 安全租户去重 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-007 | 断点续传 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 3 |
| FR-MV-008 | 增量下载 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 1 |
| FR-MV-009 | 原件不可变 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 1 |
| FR-MV-010 | 媒体探测 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-011 | 可扩展元数据 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 1 |
| FR-MV-012 | 媒体适配器统一接口 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 1 |
| FR-MV-013 | 图像版本差异 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 1 |
| FR-MV-014 | 视频时间线差异 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-015 | 音频版本差异 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 2 |
| FR-MV-016 | 三维资产差异 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 2 |
| FR-MV-017 | 安全合并能力声明 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 1 |
| FR-MV-018 | 资源依赖DAG | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 3 |
| FR-MV-019 | 多文件原子资源快照 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 1 |
| FR-MV-020 | 媒体派生缓存隔离 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 1 |
| FR-MV-021 | 隔离媒体解析 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 2 |
| FR-MV-022 | 媒体权限和分享 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-023 | 资产搜索索引 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 1 |
| FR-MV-024 | 冲突可视化 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 2 |
| FR-MV-025 | 作品来源与授权 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 2 |
| FR-MV-026 | 可验证媒体导出 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-027 | 存储逻辑与业务版本隔离 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 2 |
| FR-MV-028 | 安全垃圾回收 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 2 |
| FR-MV-029 | 块级迁移 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 2 |
| FR-MV-030 | 媒体协作事件 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| FR-MV-031 | 批量导入与目录映射 | BD-MINIOS3-012 | DD-MINIOS3-017 | 1 / 1 / 1 |
| FR-MV-032 | 面向生产的预览代理 | BD-MINIOS3-012 | DD-MINIOS3-018 | 1 / 1 / 1 |
| FR-MV-033 | 复杂工程资源类型 | BD-MINIOS3-012 | DD-MINIOS3-015 | 1 / 1 / 1 |
| FR-MV-034 | 资产语义标签 | BD-MINIOS3-012 | DD-MINIOS3-016 | 1 / 1 / 1 |
| NFR-GG-001 | 100TB规模恢复一致性 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-002 | 版本图可追溯 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-003 | 块级增量传输收益 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-004 | 大媒体常数级内存 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-005 | 资产历史查询 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-006 | 提交与内容强一致 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-007 | 离线冲突零静默丢失 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-008 | 媒体插件沙盒边界 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 2 |
| NFR-GG-009 | 细粒度撤权 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-010 | 格式适配器能力清单 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-011 | 供应链许可证合规 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
| NFR-GG-012 | 数据格式可移植 | BD-MINIOS3-012 | DD-MINIOS3-019 | 0 / 0 / 1 |
