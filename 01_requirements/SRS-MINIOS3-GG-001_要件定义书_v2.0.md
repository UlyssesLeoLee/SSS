# SRS-MINIOS3-GG-001 — GitGit + MinIOS3 多媒体版本管理系统要件定义书

> v2.0 / 2026-10-26 / 要件定义评审草案（未开发/未测试）｜IPA 共通フレーム2013 / 非機能要求グレード2018 的结构参照，不属于IPA官方认证。

## 0. 产品及权威边界

**唯一产品主目标：** 自研 GitGit 的多媒体版本管理平台。GitGit Rust Core 对 Commit/Ref/Tree/AssetRevision/协作流程拥有权威；MinIOS3 是不可变内容/分块/分布式存储和S3兼容互换引擎。Git LFS与标准Git仅作为可选互操作协议，不得变成业务主路径。

**一致性约束：** GitGit Ref更新采用 expected-old-id CAS；新Commit引用的所有Manifest/Chunk必须先由MinIOS3持久化并签发受信Receipt；索引/预览允许最终一致，但已ACK提交不得引用缺失内容。

## 1. 范围与阶段

| 阶段 | 目标 | 验收定义 |
|---|---|---|
| E0 | GitGit原生版本图、分支/Commit/Checkout、媒体CAS分块增量、S3基础兼容、单机/联邦 | 真实10GiB样本链路可执行，完整哈希复原、断点续传和并发Ref CAS |
| E1 | 资源关系与Review/协作锁/结构化合并、Native HA、WORM、审计、预览、Git LFS兼容 | 企业S3+版本强一致、分布式故障演练、真实GitGit集成验证 |
| E2 | 音视频/3D深度差异、生产专用插件、标准Git迁移 | 可解释的媒体语义差异和完整许可证/格式兼容矩阵 |

## 2. 用户角色与业务用例

| UC | 角色 | 场景 | 完成条件 | 需求关联 |
|---|---|---|---|---|
| UC-01 | 开发者 | 在GitGit仓库提交4GB视频 | GitGit原生上传Manifest、提交签名Receipt并完成Ref CAS；下载精确还原，LFS为兼容用例 | FR-GG-005 / FR-MV-007 / FR-MV-019 |
| UC-02 | 美术 | 比较GitGit中两个Commit的媒体变化 | 展示原媒体、版本源和可用对比，失败不修改原文件。 | FR-GG-008 / FR-MV-013 |
| UC-03 | 管理员 | 从MinIO S3迁移到AWS S3 | Dry-run差异、传输、校验、切换、回滚；原客户端端点保持稳定。 | FR-XCHG-006 / FR-XCHG-008 / FR-XCHG-009 |
| UC-04 | 运维 | 单节点损坏和数据自愈 | 可用性满足故障域承诺，自愈后摘要一致。 | FR-NATIVE-002 / FR-NATIVE-004 |
| UC-05 | 合规员 | 对审计资料设置法律保留 | 锁定版本不能被普通管理员删除，审计证明完整。 | FR-S3-012 / FR-SEC-007 |
| UC-06 | 审计人员 | 追踪GitGit提交的全部媒体内容 | 返回ref、commit、OID、S3版本、授权与操作历史。 | FR-GG-002 / FR-MV-027 |
| UC-07 | 开发者 | 用Boto3通过统一S3端点获取媒体 | 现有S3 SDK不需要专用客户端。 | FR-S3-002 / FR-XCHG-003 |
| UC-08 | 管理员 | 后端不具WORM能力但项目强制WORM | 路由/迁移操作被阻断，记录明确原因。 | FR-XCHG-004 / FR-S3-012 |
| UC-09 | 维护者 | 删除分支后清理孤儿媒体 | 仅回收不可达、已过宽限期且不受保留策略保护的媒体。 | FR-GG-016 / FR-MV-028 |
| UC-10 | 制作团队 | 多用户使用GitGit锁协作编辑3D模型 | 通过LFS锁阻止被保护路径并发覆盖。 | FR-GG-009 / FR-MV-016 |
| UC-11 | 运维 | 大规模后端限流和重试 | 有背压与指数退避，对其他租户影响受限。 | FR-XCHG-015 / NFR-PER-006 |
| UC-12 | 运维 | 从备份重建元数据控制面 | 恢复Git到OID映射、权限及桶配置，进行内容核验。 | FR-OPS-012 / NFR-AVA-003 |
| UC-GG-01 | 美术师 | 修改10GiB视频并提交GitGit分支 | 增量上传缺失块、签名确认、提交后可还原全部字节 | FR-GG-001 / FR-MV-004 / FR-MV-007 |
| UC-GG-02 | 声音制作 | 比较两个配音版本 | 波形、区间和声道差异定位，未知格式安全降级 | FR-MV-015 / FR-MV-024 |
| UC-GG-03 | 3D美术 | 比较模型材质/网格/动画改动 | 3D差异可定位、关键变化具备来源证明 | FR-MV-016 / FR-MV-018 |
| UC-GG-04 | 游戏团队 | 修改贴图影响多个模型 | 依赖图定位所有受影响的AssetRevision | FR-MV-018 / FR-GG-015 |
| UC-GG-05 | 策划 | 创建分支、并行修改并合并 | 有冲突时禁止静默覆盖，结果含审批/决议记录 | FR-GG-003 / FR-GG-012 / FR-GG-013 |
| UC-GG-06 | 移动办公者 | 离线编辑并按需同步媒体 | 冲突显式提示、未修改块不重复上传 | FR-GG-010 / FR-GG-011 / FR-MV-007 |
| UC-GG-07 | 管理员 | 从一种S3后端切换并恢复历史仓库 | 所有受保留Commit完整检出且S3物理版本迁移不影响GitGit ID | FR-MV-027 / FR-MV-029 / FR-GG-020 |
| UC-GG-08 | 安全审计 | 阻断不受信插件读取保密视频 | 隔离Worker无权限，拒绝详情审计可查 | FR-MV-021 / NFR-GG-008 |
| UC-GG-09 | 主美 | 审批多个媒体资产后发布版本 | 未审批资源阻断受保护分支发布，审批记录不可篡改 | FR-GG-014 / FR-MV-025 |
| UC-GG-10 | 运维 | 误删分支并发执行GC后恢复 | 受保护历史引用内容未被提前删除 | FR-GG-016 / FR-MV-028 |

## 3. 功能要件一覧（機能要求）

ID继承规则：原有104条FR不删不重号，其中15条依据变更管理修订；新需求用`FR-GG-*`与`FR-MV-*`。`status`为待评审，不能解释为实现完成。

| Requirement ID | 阶段 | 优先级 | 名称 | 要求 | 可测试接受条件 |
|---|---|---|---|---|---|
| FR-CORE-001 | E0 | MUST | 独立部署 | 允许内网/专有云/Kubernetes部署，不依赖AWS/MinIO在线控制面；GitGit原生版本管理与MinIOS3对象存储在断外网环境可运行，外部Git/LFS只作为可选兼容。 | 断开公网后，GitGit原生提交、版本回退、媒体分块上传/下载、MinIOS3 PUT/GET/LIST和管理可正常完成；禁用LFS适配仍通过。 |
| FR-CORE-002 | E1 | MUST | Native 与 Federation 运行模式 | 具备原生分布式对象存储模式（Native）及对已有 S3 后端统一接入模式（Federation），模式及风险在控制台显式区分。 | 两种部署模式均通过 S3 核心兼容测试；模式切换不得改变现有对象内容。 |
| FR-CORE-003 | E0 | MUST | 租户层级 | 支持组织、项目、存储空间（Bucket）、资源、成员、角色的层级治理，组织之间默认隔离。 | 租户 A 凭据无法枚举或读取租户 B 的任何资源；隔离自动测试100%通过。 |
| FR-CORE-004 | E1 | MUST | 租户资源计量和配额 | 按租户/项目/桶统计逻辑大小、物理占用、请求量、版本和账单量，并可配置软硬配额。 | 超出硬配额返回明确错误，管理员可追踪来源；计量误差可核对。 |
| FR-CORE-005 | E0 | MUST | 存储命名空间和资源映射 | 提供稳定的对外Bucket/Key命名空间；内部可映射不同后端与故障域，重路由不改变客户端资源标识。 | 迁移后使用原端点、原 Bucket/Key 可以正确读取原版本。 |
| FR-CORE-006 | E1 | MUST | 配置版本与回滚 | 所有路由、策略和生命周期配置需要版本记录、影响预览、审计与安全回滚。 | 错误路由配置可恢复上一生效版本，不改变已确认对象。 |
| FR-CORE-007 | E1 | MUST | 多区域和数据驻留策略 | 按组织/桶配置允许的地区、数据驻留与禁止跨境复制规则，默认拒绝不满足策略的目标后端。 | 禁止区域间转移时，对应迁移/复制任务无法启动并写入审计记录。 |
| FR-CORE-008 | E1 | MUST | 能力发现接口 | 公开服务能力信息，包括Native/Federation、S3 API支持、锁定策略、最大对象、校验与一致性级别。 | API和UI可取得对应版本的能力清单；不支持功能明确返回非兼容错误。 |
| FR-S3-001 | E0 | MUST | Bucket 生命周期 | 支持 CreateBucket、HeadBucket、ListBuckets、DeleteBucket 与地域约束。 | 创建、列举、空桶删除、权限错误及地域配置测试通过。 |
| FR-S3-002 | E0 | MUST | 标准对象读写 | 支持 PutObject/GetObject/HeadObject/DeleteObject、元数据、Content-Type、ETag与基本条件请求。 | 主流 S3 SDK 的上传、下载、覆盖、条件请求符合兼容基线。 |
| FR-S3-003 | E0 | MUST | 对象列举 | 支持 ListObjectsV2、prefix、delimiter、continuation-token，结果与权限范围一致。 | 千万级键空间分页不漏不重；与提交确认的写入符合指定一致性。 |
| FR-S3-004 | E0 | MUST | 分段上传 | 实现 CreateMultipartUpload、UploadPart、ListParts、CompleteMultipartUpload、AbortMultipartUpload 及失效分段清理。 | 分段乱序、断线续传、重复Complete、Abort及并发重试通过。 |
| FR-S3-005 | E1 | MUST | 版本控制 | 支持 Bucket Versioning、VersionId、ListObjectVersions、Delete Marker、按版本读取/删除及状态转移规则。 | 覆盖与删除后，历史版本可依授权恢复，版本标识稳定且唯一。 |
| FR-S3-006 | E0 | MUST | 签名鉴权 | 支持 AWS Signature Version 4、请求时钟误差限制、Canonical Request 与拒绝重放策略。 | 官方AWS SDK认证场景及坏签名、过期时间戳均有正确响应。 |
| FR-S3-007 | E0 | MUST | 临时预签名 | 支持预签名 GET/PUT/HEAD 链接、有效期、权限边界与撤销策略。 | 过期、越权、撤销后无法访问；范围约束与审计关联正确。 |
| FR-S3-008 | E0 | MUST | HTTP Range 与流式传输 | 支持 Range GET、If-Match、If-None-Match、分块流式传输，适配视频拖动与大文件断点续传。 | 对≥20GiB对象进行Range读取，不整体加载内存，返回正确206与校验。 |
| FR-S3-009 | E1 | MUST | 对象属性及标签 | 支持用户元数据、标签、CopyObject、Tagging、标准对象键编码规则。 | 元数据/标签在复制迁移后按策略完整保留。 |
| FR-S3-010 | E1 | MUST | 分层策略 | 支持 Bucket Policy、IAM式授权映射、可配置ACL兼容模式；明确不支持的ACL语义禁止静默降级。 | 默认拒绝、多租户隔离、策略冲突和SDK常见操作矩阵通过。 |
| FR-S3-011 | E1 | MUST | 生命周期管理 | 支持期限到期、过期非当前版本、未完成Multipart清理及按对象标签/前缀过滤。 | TTL到期任务正确执行；被保留/保护的版本不被误删。 |
| FR-S3-012 | E1 | MUST | Object Lock | 支持 Object Lock、Governance/Compliance模式、Retention与Legal Hold，版本级保护不可被普通管理员API绕过。 | WORM对覆盖、版本删除、撤销保留、时钟回拨等攻击测试拒绝；独立审计完成。 |
| FR-S3-013 | E1 | MUST | 事件通知 | 发布 PUT、DELETE、版本变化等标准事件到Webhook/队列，对失败投递可重试并去重。 | 消费者重复收到事件也能幂等处理；失败可重放。 |
| FR-S3-014 | E1 | MUST | SSE/KMS兼容 | 支持 SSE-S3、SSE-KMS 的请求和响应语义；未达成的加密模式单独标注，禁止假成功。 | 使用配置KMS读写正常，密钥拒绝/轮换行为受测。 |
| FR-S3-015 | E1 | MUST | Object Checksum | 支持标准校验头与全对象及分段校验，可区分 ETag 与内容摘要，不得默认等同于 MD5。 | 篡改块校验失败；迁移前后SHA-256一致。 |
| FR-S3-016 | E2 | MUST | S3 Batch 操作 | 支持批量对象操作任务及每条结果报告，长作业可暂停、重试、恢复。 | 10万对象批量任务中断重启不丢状态、无重复破坏性变更。 |
| FR-S3-017 | E1 | MUST | S3 兼容性清单 | 发布逐API、SDK语言、后端模式、版本、已知差异及回归结果清单。 | 发布版本附机器可读兼容清单，并对宣称支持的API自动回归。 |
| FR-S3-018 | E1 | MUST | 虚拟主机与路径风格 | 支持 path-style 和可配置 virtual-hosted-style endpoint、TLS 域名与bucket命名约束。 | 两种地址形式在合法桶名、特殊字符、代理TLS下通过。 |
| FR-S3-019 | E1 | MUST | 对象幂等与并发条件 | 以条件写、防覆盖、幂等键或可追踪操作令牌保护重试与竞态；遵守S3兼容语义。 | 重试不会生成损坏版本；并发相同键操作呈现可解释顺序。 |
| FR-GIT-001 | E1 | MUST | Git LFS Batch API | 作为对现有Git LFS客户端的兼容接口支持Batch upload/download/verify。GitGit原生媒体协议是E0主路径；LFS指针不是GitGit原生版本记录。 | LFS兼容客户端传输与SHA校验通过；禁用LFS兼容服务不影响GitGit原生E0媒体版本链路。 |
| FR-GIT-002 | E1 | MUST | Git LFS Locking API | 实现Git LFS Locking兼容适配，映射到GitGit原生资产锁，但GitGit锁状态为权威。 | GitGit锁与LFS锁查询一致；未启用LFS时原生协作锁仍有效。 |
| FR-GIT-003 | E1 | MUST | 标准Git独立性 | GitGit拥有自研Commit/Ref/Tree和版本模型；标准Git对象格式与Git wire protocol是否兼容由独立互操作适配层声明，不将第三方Git内核作为依赖。 | 关闭标准Git互操作插件后GitGit原生版本管理仍能提交、分支、比较和恢复。 |
| FR-GIT-004 | E0 | MUST | 内容寻址 | 原始媒体由MinIOS3以SHA-256和强完整性收据做内容寻址；Git LFS OID映射为可选兼容标识，不能替代原生ContentManifestId。 | 同字节同策略域复用且不改变原件；租户隔离，读取重新校验。 |
| FR-GIT-005 | E0 | MUST | 引用完整性检查 | GitGit自研Ref CAS提交前校验所有引用的AssetRevision/ContentManifest已持久化，采用签名准备票据及服务端栅栏；LFS验证仅属于兼容路径。 | 缺块、旧epoch、非法ACL或过期票据均拒绝提交；旧Ref保持不变。 |
| FR-GIT-006 | E0 | MUST | 版本关联索引 | 建立GitGit Repo/Commit/Tree/AssetRevision/Manifest/Chunk/S3逻辑版本/物理位置的版本谱系；Git LFS OID仅为可选映射。 | Commit→资产→内容块→物理存放可逆向追踪且hash验证一致。 |
| FR-GIT-007 | E0 | MUST | Git分支/Tag与媒体历史 | GitGit原生分支、标签、Snapshot及媒体历史视图为权威，不以S3 VersionId或LFS OID替代提交身份。 | 创建、切换、比较、回退GitGit分支正确还原媒体内容；S3版本ID差异不影响历史。 |
| FR-GIT-008 | E0 | MUST | Git身份继承 | GitGit作为第一方版本引擎共享服务鉴权与仓库授权策略，MinIOS3仍实施独立数据面权限校验；外部Git鉴权为兼容适配。 | 撤销GitGit权限后新的原生读取及签名URL请求被拒；跨租户不可枚举CAS。 |
| FR-GIT-009 | E0 | MUST | Git事件接入 | GitGit自研事务事件/outbox提供可信提交事件、签名校验和幂等同步；Webhook为互操作适配。 | 重复、乱序、遗漏后通过快照watermark对账；GC对未证完整可达性拒删。 |
| FR-GIT-010 | E1 | MUST | 媒体垃圾回收安全 | GitGit原生可达性图及租户/分支/Tag/快照/法务保留决定安全回收；MinIOS3独立执行双阶段物理GC。 | 在并发新引用、迁移、快照与合法保留期间零误删。 |
| FR-GIT-011 | E1 | MUST | 离线和部分克隆 | GitGit原生稀疏检出与部分媒体下载、离线工作区增量同步；Git LFS smudge仅兼容。 | 离线提交后重连按冲突策略重新校验；只下载请求Chunk并保持SHA一致。 |
| FR-GIT-012 | E1 | MUST | 仓库配额和限流 | 按仓库/组织限制媒体大小、并发、带宽、请求量并支持覆盖审批。 | 超配额上传在写入前可预测拒绝，已存在历史不被误删。 |
| FR-GIT-013 | E0 | MUST | 提交一致性补偿 | GitGit自研Ref/Commit原子CAS与MinIOS3持久化内容收据通过Prepare→Commit→Confirm协议衔接；禁止假设跨服务ACID。 | 断电、超时、重复请求不会产生可见缺块提交；补偿可恢复且GC保护。 |
| FR-GIT-014 | E2 | MUST | 现有Git迁移 | 支持从标准Git/Git LFS仓库迁入资产及历史的可选兼容工具，须校验目标GitGit版本图与内容SHA映射。 | 导入后保留历史关系与内容摘要且输出无法映射特性的差异报告。 |
| FR-MEDIA-001 | E0 | MUST | 多媒体资产分类 | 支持图像、视频、音频、3D、CAD、PSD、工程归档和任意二进制；未知类型不妨碍原样存储。 | 至少PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP完成端到端验证。 |
| FR-MEDIA-002 | E0 | MUST | 资产元数据 | 保存媒体类型、大小、时间、尺寸、时长、哈希、作者、许可、标签、项目与版本来源。 | 元数据与Git Commit及OID可以双向检索。 |
| FR-MEDIA-003 | E0 | MUST | 内容零改写 | 原始文件不得被缩略图、转码或元数据处理流程覆盖；派生资源另存可回溯。 | 下载SHA-256与原上传文件相同。 |
| FR-MEDIA-004 | E1 | MUST | 异步预览 | 按媒体能力生成图片缩略图、视频关键帧、波形和3D预览；失败不影响原文件访问。 | 预览出错时下载与版本读取不受影响，且有失败原因。 |
| FR-MEDIA-005 | E1 | MUST | 版本差异辅助 | GitGit原生媒体Adapter提供差异接口：图像叠加、音视频时间码比较、3D拓扑/材质差异；不支持安全合并时明确拒绝。 | 通过媒体专用golden fixture识别变化，无法识别时回退hash/size而不宣称语义相等。 |
| FR-MEDIA-006 | E1 | MUST | 资产搜索 | 按项目、扩展名、标签、作者、Git Ref/Commit、上传时间和SHA-256等查询。 | 指定索引规模下按分页检索，结果遵循权限隔离。 |
| FR-MEDIA-007 | E1 | MUST | 媒体预览安全 | 所有解析/转码任务必须沙盒隔离、限制CPU/RAM/执行时间、过滤外部资源与恶意内容。 | 构造异常媒体文件不能访问宿主机密钥或其他租户对象。 |
| FR-MEDIA-008 | E1 | MUST | 预览内容与源关联 | 缩略图、代理视频、波形、3D转换结果均保存源OID、工具版本和生成参数。 | 源变化触发可识别的重新生成；源内容保持不可变。 |
| FR-MEDIA-009 | E1 | MUST | 媒体交付 | 可为受权用户提供Range/CDN缓存/限速/临时链接，支持跨区域媒体流式读取。 | 断网后续传与视频Seek测试通过；缓存不泄漏租户数据。 |
| FR-MEDIA-010 | E2 | MUST | 许可与溯源 | 对商业化素材维护创作者、版权授权、来源、限制及审批状态，可阻止未经授权的发布。 | 受限制素材无法被标记为可公开发布。 |
| FR-XCHG-001 | E0 | MUST | S3后端适配 | 支持AWS S3与符合声明兼容集的第三方S3（含MinIO）作为可配置后端。 | 同一测试数据在至少AWS S3与另一个自建S3实现上完整往返。 |
| FR-XCHG-002 | E2 | MUST | 非S3供应商适配 | 以插件扩展Azure Blob/GCS等；必须显式标注功能差异和支持边界。 | 增补适配器无需更改S3客户端公开接口。 |
| FR-XCHG-003 | E0 | MUST | 统一端点 | 同一客户端AccessKey和Endpoint能访问路由到不同提供商的桶。 | 原有客户端仅改Endpoint/密钥即可访问受支持S3 API。 |
| FR-XCHG-004 | E0 | MUST | 后端能力矩阵 | 适配器声明Versioning、WORM、SSE、复制、一致性、Multipart、Event等特性。 | 要求WORM而后端不满足时禁止部署，不允许虚假成功。 |
| FR-XCHG-005 | E1 | MUST | 存储策略与放置 | 按租户、大小、地域、价格、延迟、保留级别及访问频度配置落点和副本策略。 | Dry-run输出每条规则的匹配原因和成本估计。 |
| FR-XCHG-006 | E0 | MUST | 迁移作业 | 支持源→目标全量/增量迁移、断点续传、进度、限流和错误清单。 | 百万对象迁移中断后恢复，不漏文件；核验摘要与元数据。 |
| FR-XCHG-007 | E0 | MUST | 迁移预演 | 执行前展示对象数、总字节、估算外传费、API差异、版本风险和预计不可用窗口。 | 存在不兼容Object Lock时必须报阻断项。 |
| FR-XCHG-008 | E0 | MUST | 无损切换 | 支持冻结窗口或复制追平+切换+回滚流程，业务端点和Git OID不变。 | 生产演练迁移失败可回退到源端，用户已确认写入不丢失。 |
| FR-XCHG-009 | E1 | MUST | 双端校验 | 比较SHA-256/长度/内容版本/标签/权限/保留策略，给出不兼容项清单。 | 对人为损坏和缺失分别准确告警。 |
| FR-XCHG-010 | E1 | MUST | 连续复制 | 提供带重试、延迟、死信队列的跨后端单向复制及删标志治理。 | 乱序和网络分区不产生静默覆盖；复制滞后指标可见。 |
| FR-XCHG-011 | E1 | MUST | 冲突控制 | 跨后端写入须定义唯一主写入者或明确的冲突解决策略，默认拒绝未定义的双主。 | 并发写同键情况下不允许最后写入静默掩盖冲突。 |
| FR-XCHG-012 | E1 | MUST | 读取故障转移 | 灾备副本符合可读性和一致性要求时才切换，显示RPO范围和复制滞后。 | 故障转移返回一致性状态；未同步数据不得误报存在。 |
| FR-XCHG-013 | E1 | MUST | 后端出口开放 | 提供对象、元数据、版本映射与迁移清单的开放格式导出，不能将数据锁定在私有数据库。 | 第三方工具可从导出的数据重建完整键空间及版本关系。 |
| FR-XCHG-014 | E2 | MUST | 成本与计量 | 按后端记录存储、请求、网络出站、复制及冗余成本并提供项目级报告。 | 成本报表能与供应商账单进行差额核对。 |
| FR-XCHG-015 | E1 | MUST | 底层供应商异常隔离 | 后端限流、超时、签名异常、配额不足和地区不可达需归因、隔离和熔断。 | 一个后端故障不会拖垮其它桶的正常请求。 |
| FR-NATIVE-001 | E1 | MUST | 多节点横向扩展 | Native模式支持无共享单点架构并支持水平添加节点和容量。 | 扩容演练不需要停掉全部S3服务。 |
| FR-NATIVE-002 | E1 | MUST | 纠删码/数据保护 | Native模式支持可配置的数据校验及多磁盘/节点故障容忍方案；编码策略写入存储池属性。 | 配置容错范围内故障可读取完整数据，超出范围进入安全只读/错误状态。 |
| FR-NATIVE-003 | E1 | MUST | 故障域意识 | 支持机架/宿主机/可用区等故障域约束，避免数据碎片过于集中。 | 放置审计证明碎片满足故障域策略。 |
| FR-NATIVE-004 | E1 | MUST | 节点自愈与巡检 | 节点掉线重入后自动扫描与重建损坏/丢失块，支持限速并显示风险。 | 混合故障注入后数据摘要不变，进度和剩余风险可查询。 |
| FR-NATIVE-005 | E1 | MUST | 存储一致性语义 | 对已确认单桶写入提供强读后写与原子单键替换；异步跨站副本单独声明一致性。 | ACK后GET/HEAD/LIST（在规定作用域）立即可见，不出现半对象。 |
| FR-NATIVE-006 | E1 | MUST | 数据与控制面故障隔离 | 节点/磁盘/控制服务失效时，管理接口错误不能导致已确认对象损坏。 | 单点注入与恢复试验通过。 |
| FR-NATIVE-007 | E1 | MUST | 容量水位保护 | 有高低水位线、保留空间、写入背压、容量风险告警，禁止盘满后静默损坏。 | 容量接近上限能预测并安全拒绝写入。 |
| FR-NATIVE-008 | E1 | MUST | 数据损坏检测 | 端到端校验、后台Scrubbing、bit-rot检测与修复记录。 | 注入存储位错误后检测到并在冗余足够时自愈。 |
| FR-NATIVE-009 | E1 | MUST | 故障演练接口 | 提供可自动化的断节点、网络分区、磁盘满、时钟漂移和恢复验证清单。 | 每个发布候选版本执行故障注入测试。 |
| FR-SEC-001 | E1 | MUST | SSO与身份联邦 | 支持OIDC/OAuth2及企业IdP；按需支持SAML通过联邦代理适配。 | SSO登录/登出/禁用账号后会话生效性经测试。 |
| FR-SEC-002 | E1 | MUST | IAM/RBAC/ABAC | 支持基于角色、资源、条件的最小权限与权限继承；所有功能默认拒绝。 | 尝试越权访问、横向提权和策略覆盖均失败。 |
| FR-SEC-003 | E1 | MUST | 短期凭据 | 提供STS兼容短期访问、AccessKey轮换、密钥回收、过期和作用域限制。 | 过期Token和已撤销AccessKey被拒绝并审计。 |
| FR-SEC-004 | E1 | MUST | 传输加密 | 所有业务外部接口默认TLS；集群东西向和管理面使用mTLS或等效身份认证。 | 未加密入口默认关闭；TLS配置可扫描验证。 |
| FR-SEC-005 | E1 | MUST | 静态加密与密钥管理 | 支持信封加密、KMS/HSM集成、密钥轮换与租户范围隔离。 | KMS故障不会造成明文回退，轮换后的旧数据可访问。 |
| FR-SEC-006 | E1 | MUST | 审计追踪 | 记录谁、何时、从何处对哪个对象/版本作何动作及结果；记录策略、密钥、部署、迁移和Git ref变化。 | 追踪一个Git Commit至对应OID和S3 Version与访问日志。 |
| FR-SEC-007 | E1 | MUST | 不可篡改审计 | 安全敏感审计必须转存独立WORM/外部系统并具链式校验或签名防篡改机制。 | 系统管理员不能通过普通运维权限覆盖已封存审计记录。 |
| FR-SEC-008 | E1 | MUST | 恶意软件与内容控制 | 可选AV/DLP扫描与恶意文件隔离；不得阻断无关租户的核心存储。 | 恶意媒体可隔离并可追查，原始SHA-256和隔离动作有证据。 |
| FR-SEC-009 | E1 | MUST | 管理面防护 | MFA、会话过期、高风险动作二次确认、四眼审批可配置。 | 删除存储池、强制解锁、迁移切换触发审批规则。 |
| FR-SEC-010 | E1 | MUST | 安全加固与供应链 | SBOM、签名发行物、依赖漏洞扫描、凭据扫描、最小容器权限、密钥不落日志。 | 关键漏洞阻止正式发布，SBOM可下载。 |
| FR-SEC-011 | E1 | MUST | 合法持有与数据隔离 | 支持租户导出/删除/保留策略及指定辖区的数据驻留限制。 | 租户删除前检测WORM和Git活跃引用，依法合规执行。 |
| FR-SEC-012 | E1 | MUST | 秘密管理 | 所有存储适配器凭据在秘密存储中加密保存，支持热轮换和最小授权。 | 数据导出和日志不包含明文AccessKey/SecretKey。 |
| FR-SEC-013 | E1 | MUST | 勒索防护 | 通过保留保护、版本恢复、异地不可变副本及异常删除/下载告警减轻勒索风险。 | 批量误删后可以按保留策略恢复；模拟攻击产生报警。 |
| FR-SEC-014 | E1 | MUST | 合规声明边界 | 只有通过外部评估后才能声称法规合规或WORM认证；系统界面不得误标。 | 发布材料包含适用范围、例外、证据和未认证声明。 |
| FR-OPS-001 | E0 | MUST | Web管理控制台 | 提供存储池、桶、对象、版本、Git仓库、媒体库、用户、权限、任务及日志的统一导航。 | 任务列表和对象详情三步内可进入，角色不可见不属于自己的资源。 |
| FR-OPS-002 | E1 | MUST | 版本关系可视化 | 同屏呈现Git Commit↔LFS OID↔Bucket/Key↔VersionId与副本位置。 | 定位一项历史资源无需理解后端私有路径。 |
| FR-OPS-003 | E1 | MUST | 迁移与复制工作台 | 查看拓扑、队列、滞后、数据校验、不兼容特性、暂停恢复、回滚与失败明细。 | 操作可审计；危险动作有影响预览。 |
| FR-OPS-004 | E1 | MUST | 媒体浏览器 | 支持目录前缀视图、标签检索、缩略图/元数据/版本/下载授权。 | 图像、视频、3D预览异常不阻断下载。 |
| FR-OPS-005 | E1 | MUST | 多语言 | 前端与文档默认zh-CN，提供ja-JP/en-US语言包、国际化时间和错误码。 | 语言切换不改变权限/对象数据；日语信息自然一致。 |
| FR-OPS-006 | E2 | MUST | 无障碍与认知负荷 | 优先简洁任务流程，复杂管理字段按需展开，键盘可访问，危险动作明确区分。 | 键盘完成桶创建、恢复版本和权限查询，核心流程可用性评审通过。 |
| FR-OPS-007 | E1 | MUST | CLI与配置即代码 | 提供跨平台CLI和声明式YAML配置，支持dry-run/validate/diff/apply。 | 操作与Web实现相同权限与审计策略。 |
| FR-OPS-008 | E2 | MUST | API文档与SDK示例 | 提供OpenAPI管理接口文档、S3适配矩阵、Git LFS协议说明和Rust/Go/JS/Python SDK例子。 | 新开发者按文档完成上传、Git LFS和迁移样例。 |
| FR-OPS-009 | E1 | MUST | 审计查询与报告 | 支持按用户、项目、仓库、Commit、版本、时间查询并导出受控审计报告。 | 报告可重现追查一次管理员动作与Git修改。 |
| FR-OPS-010 | E0 | MUST | 监控与告警 | 提供Prometheus/OpenTelemetry指标与追踪，容量、错误率、延迟、复制、恢复与KMS状态告警。 | Grafana/兼容仪表板可读取关键指标并按阈值触发。 |
| FR-OPS-011 | E1 | MUST | 零停机滚动升级 | 定义支持N/N-1版本兼容矩阵和滚动升级回退程序。 | 在允许的单节点轮替中读写持续；失败能按预案恢复。 |
| FR-OPS-012 | E1 | MUST | 备份与恢复 | 完整备份元数据、身份策略、映射表、KMS依赖说明和灾备校验；支持定期恢复演练。 | 仅从备份与对象数据可复原Git映射和用户可读对象。 |
| FR-OPS-013 | E1 | MUST | 异常与根因信息 | 统一错误码、关联ID、错误上下文与可执行修复建议；不泄露凭据。 | S3错误+后台trace可串联到具体后端与操作。 |
| FR-OPS-014 | E1 | MUST | 运维 runbook | 提供扩容、换盘、证书/密钥轮换、恢复、恢复演练、容量满及数据搬迁SOP。 | 演练人员不依赖开发者介入完成预定操作。 |
| FR-OPS-015 | E1 | MUST | 服务水平报告 | 对外发布SLO、错误预算、重大事故、事件响应指标及兼容性变更。 | 月报按原始监控数据复算一致。 |
| FR-GG-001 | E0 | MUST | GitGit仓库权威 | GitGit Rust引擎独立持有Repository/Ref/Commit/Tree/AssetRevision提交权威，不依赖Gitoxide或外部Git服务器。 | 停用所有外部Git兼容接口，仍能完成创建仓库、提交、分支与历史恢复。 |
| FR-GG-002 | E0 | MUST | 不可变提交图 | Commit以固定序列化字段、parents、root-tree与manifest摘要形成不可变有向无环图。 | 同输入稳定得到同Commit ID；不能通过正常API改写历史对象。 |
| FR-GG-003 | E0 | MUST | 原子Ref CAS | 分支/标签更新必须绑定expected-old-id与RefRevision，在元数据事务中原子提交。 | 两名并发写者仅一个成功；失败者看见明确冲突且不产生部分Ref。 |
| FR-GG-004 | E0 | MUST | 仓库快照 | 提交快照记录资产路径、AssetId、RevisionId、ManifestId；恢复不依赖当前目录。 | 重命名/删除之后checkout旧Commit按字节还原全部文件。 |
| FR-GG-005 | E0 | MUST | 两阶段提交门禁 | GitGit在提交前要求每份新内容具备MinIOS3签名持久化收据并校验policy/placement epoch。 | 缺片或Receipt过期时拒绝Ref发布，且保留可补偿事务。 |
| FR-GG-006 | E0 | MUST | 幂等确认与Outbox | Commit事件在GitGit事务内写入outbox，事件消费者按event_id及sequence幂等处理。 | 断网+重放+乱序事件后两侧谱系最终对账一致，不产生重复版本。 |
| FR-GG-007 | E0 | MUST | 树级变更与重命名 | 支持路径新增删除重命名、资产身份稳定关联和目录差异计算。 | 重命名保留AssetId及全部历史；路径冲突不得静默覆盖。 |
| FR-GG-008 | E0 | MUST | 分支差异查询 | 提供Commit/Branch/Tag双边比较，返回路径、资产、内容和依赖差异。 | 带1000个资产的样本差异结果可按AssetId/RevisionId复算。 |
| FR-GG-009 | E1 | MUST | 原生资产锁 | GitGit支持按资产/路径/分支作用域的租约锁、持有者、到期策略及强制审批。 | 并发锁冲突拒绝覆盖，强制解锁有审计并更新fencing token。 |
| FR-GG-010 | E1 | MUST | 离线工作区日志 | 离线修改保留本地变更journal、基线Commit和重放请求ID。 | 断网修改恢复后不误覆盖远端并提示冲突。 |
| FR-GG-011 | E1 | MUST | 稀疏工作区与懒加载 | 按目录/资产/媒体片段规则选择性checkout及按需hydrate。 | 未选中的20GiB媒体不下载，选中内容下载SHA与Manifest一致。 |
| FR-GG-012 | E1 | MUST | 三方合并引擎 | 区分可自动合并的文本/结构化资源与不可安全合并的二进制资源。 | 不可安全合并时返回显式冲突，绝不自动选取后写者。 |
| FR-GG-013 | E1 | MUST | 冲突决议审计 | 人工决议提交必须保存双方Revision、冲突范围及操作者审计。 | 合并结果可复查全部parent和resolve原因。 |
| FR-GG-014 | E1 | MUST | 媒体评审流 | 允许针对Commit/AssetRevision发起评论、审批、驳回和发布门禁。 | 未获必需审批无法发布指定受保护分支。 |
| FR-GG-015 | E1 | MUST | 依赖变更影响 | Commit时记录资产依赖DAG版本，用于上游资产变更影响计算。 | 修改纹理后定位全部引用该纹理的模型且不跨分支误报。 |
| FR-GG-016 | E1 | MUST | 历史可达性水位 | GitGit暴露签名的全Ref/Tag/保护快照及Snapshot可达集供MinIOS3安全GC。 | 丢事件或正在并发恢复时GC fail-closed且不得误删仍可达块。 |
| FR-GG-017 | E0 | MUST | 组织级授权 | 仓库/分支/资产路径/敏感媒体授权由GitGit统一发令，存储数据面二次校验。 | 撤权与过期读票据不会绕开资源级ACL。 |
| FR-GG-018 | E1 | MUST | 受保护提交策略 | 支持签名提交、受保护分支、强制推送限制和受控恢复。 | 无权force-update拒绝且所有策略决策留痕。 |
| FR-GG-019 | E2 | SHOULD | 标准Git迁入隔离 | Git pack/wire、git-lfs和旧仓库迁入作为独立可选Adapter，不嵌入原生提交引擎。 | 禁用互操作插件后原生核心可运行；迁移映射差异清晰可查。 |
| FR-GG-020 | E0 | MUST | 提交与资产恢复 | 从任意受保留Commit/Tag恢复仓库完整目录及媒体内容，验证所有Manifest。 | 模拟跨存储迁移后历史恢复字节正确。 |
| FR-MV-001 | E0 | MUST | 稳定资产标识 | MediaAsset有跨路径重命名保持不变的AssetId，内容变更产生新的AssetRevisionId。 | 移动/改名不改变AssetId，更新内容必生成新Revision。 |
| FR-MV-002 | E0 | MUST | 媒体版本记录 | AssetRevision关联Commit、父Revision集合、ManifestId、MediaAdapter版本与元数据来源。 | 同一Asset存在分叉与合并时可画出完整版本DAG。 |
| FR-MV-003 | E0 | MUST | 内容清单 | ContentManifest使用版本化确定性格式定义ChunkOffset/Size/Hash/Compression及整体SHA。 | 任意读取块可按清单重组并与原始SHA-256完全一致。 |
| FR-MV-004 | E0 | MUST | 内容定义分块 | 支持FastCDC或等价CDC可调最小/平均/最大Chunk尺寸及固定大小降级模式。 | 前部插入小内容后后续大部分相同Chunk能够复用，参数与分块版本固定。 |
| FR-MV-005 | E0 | MUST | 强摘要验证 | 每块BLAKE3内部快速校验、原件SHA-256权威摘要和长度校验均须保存。 | 随机篡改任意Chunk或Manifest必被拒绝/隔离，不能返回成功ACK。 |
| FR-MV-006 | E0 | MUST | 安全租户去重 | 去重作用域默认限定加密/授权租户域，不对外暴露跨租户命中率或存在性。 | Tenant B测时/响应无法判定Tenant A内容是否存在，B无权取A的Chunk。 |
| FR-MV-007 | E0 | MUST | 断点续传 | Session创建、Chunk查询、校验提交和resume cursor支持中断恢复与重试去重。 | 20GiB模拟流断线后仅补传缺块，不重复生成同一可见Revision。 |
| FR-MV-008 | E0 | MUST | 增量下载 | 按照Manifest流式组装Range，支持稀疏检出和仅下载缺失Chunk。 | 部分请求字节/offset正确，内存占用不随文件总大小线性增长。 |
| FR-MV-009 | E0 | MUST | 原件不可变 | 任何媒体分析、合并、预览和转换都不得修改已确认原始媒体字节。 | 所有派生对象sha不同空间，重取旧Commit的原件字节完全一致。 |
| FR-MV-010 | E0 | MUST | 媒体探测 | 按字节特征而非仅扩展名识别媒体格式，记录探测版本/可信度/失败信息。 | 伪造扩展名不能自动触发高风险解析器，未知类型安全保存。 |
| FR-MV-011 | E1 | MUST | 可扩展元数据 | 支持尺寸、帧率、时长、轨道、色彩空间、骨骼、license及自定义字段有版本Schema。 | 未知字段可往返且Schema升级不破坏原始数据。 |
| FR-MV-012 | E0 | MUST | 媒体适配器统一接口 | Rust MediaAdapter可协商inspect/preview/diff/merge/validate能力与版本。 | 不支持能力返回Unsupported而非默认成功，Adapter升级不修改旧Revision。 |
| FR-MV-013 | E1 | MUST | 图像版本差异 | 提供视觉叠加、差异热区和源图对照，明确颜色空间及阈值。 | 参考图golden数据可复算热区，PSDs不支持图层时降级为渲染图比较。 |
| FR-MV-014 | E2 | SHOULD | 视频时间线差异 | 支持视频帧/镜头/时间码映射及代理视频对照，保留音视频同步关系。 | 变更片段时间码与样本golden一致，不能把非确定性转码当原内容证据。 |
| FR-MV-015 | E2 | SHOULD | 音频版本差异 | 支持波形、频谱、片段时间轴、声道/采样率变化识别。 | 参考音频能定位差异区间，未标准化采样率须显式提示。 |
| FR-MV-016 | E2 | SHOULD | 三维资产差异 | 支持Mesh/Material/Texture/Hierarchy/Animation的结构摘要与可视化对比。 | 相同拓扑不同材质及相同外观不同网格被正确区别。 |
| FR-MV-017 | E1 | MUST | 安全合并能力声明 | 按具体媒体格式/schema声明能否三方合并，未知类型仅binary-conflict决议。 | 两个不可合并二进制冲突绝不自动丢失一方修改。 |
| FR-MV-018 | E1 | MUST | 资源依赖DAG | 建立模型→纹理、工程→音频、镜头→素材等AssetRevision级有向依赖关系。 | 循环引用检测、跨Commit依赖变更反向追踪正确。 |
| FR-MV-019 | E0 | MUST | 多文件原子资源快照 | GitGit Commit绑定完整资源清单，媒体块确认在Ref发布之前。 | 断电时不会出现Commit A包含一半新素材一半未确认素材。 |
| FR-MV-020 | E1 | MUST | 媒体派生缓存隔离 | 缩略图/代理/波形按源Manifest、参数hash、Adapter版本生成可重建派生资源。 | 缓存污染或过期不影响原始数据，可清除并重建。 |
| FR-MV-021 | E1 | MUST | 隔离媒体解析 | 媒体插件/FFmpeg等高风险解析在沙盒Worker运行，禁挂载主存储密钥。 | 构造ZIP炸弹/格式畸形样本，Worker触发限额时主服务不崩溃。 |
| FR-MV-022 | E1 | MUST | 媒体权限和分享 | 按项目/资产/Revision验证播放、下载、比较和生成分享链接权限。 | 撤权后旧预览URL及Range不能继续访问受保护源。 |
| FR-MV-023 | E1 | MUST | 资产搜索索引 | 以版本级别索引作者、标签、媒体特征、依赖、审核状态与GitGit Ref。 | 权限过滤正确且展示index watermark，允许索引延迟但不伪造提交状态。 |
| FR-MV-024 | E1 | MUST | 冲突可视化 | 提供文字/图像/音视频/3D对比结果及无法比较的安全退化视图。 | 不支持内容显示元数据差异、哈希和人工冲突入口。 |
| FR-MV-025 | E2 | SHOULD | 作品来源与授权 | 维护创作者、授权范围、有效期、外部素材来源和发布用途约束。 | 授权过期的外部媒体阻断发布，下载与审计保留可授权访问。 |
| FR-MV-026 | E1 | MUST | 可验证媒体导出 | 导出Commit/AssetRevision及Manifest/Chunk摘要和来源证明包。 | 导出的包在离线验证工具中重建SHA与版本谱系一致。 |
| FR-MV-027 | E0 | MUST | 存储逻辑与业务版本隔离 | GitGit Revision与MinIOS3 CAS Manifest、S3 LogicalVersion和Provider VersionId独立建模。 | 迁移后GitGit revision不改变，即使目标Provider versionId完全不同。 |
| FR-MV-028 | E1 | MUST | 安全垃圾回收 | GitGit全量可达性、对象锁、分享链接、恢复窗口、上传事务、复制滞后决定块删除。 | 在并发checkout/restore与GC竞态中绝不删仍有引用的Chunk。 |
| FR-MV-029 | E1 | MUST | 块级迁移 | 迁移任务对Manifest涉及的Chunk执行引用保护、内容校验与mapping切换。 | 切换期间所有受保留Commit可恢复，迁移后块SHA不变。 |
| FR-MV-030 | E1 | MUST | 媒体协作事件 | 对资产评论、锁定、Revision变动和发布输出可重放有序事件。 | 单资产事件乱序处理后最终timeline与权威Commit序列一致。 |
| FR-MV-031 | E1 | MUST | 批量导入与目录映射 | 支持大型目录与百万资产索引的流式导入、断点续传和失败报告。 | 导入重试不会重复资产；目录相对路径和大小写冲突按策略报告。 |
| FR-MV-032 | E2 | SHOULD | 面向生产的预览代理 | 视频代理、缩略图、音轨片段与源AssetRevision建立独立一致关联。 | 代理生成失败不阻止GitGit Checkout；展示派生版本与处理参数。 |
| FR-MV-033 | E2 | SHOULD | 复杂工程资源类型 | Unity/Unreal/Blender/CAD适配器以Schema插件管理引用而非硬编码进CAS。 | 缺少某Adapter不影响完整二进制保存，依赖分析显式标Unsupported。 |
| FR-MV-034 | E1 | MUST | 资产语义标签 | 版本管理自定义标签、制作阶段、负责人、Review状态与媒体协作注释。 | 可查询某Commit下标签快照，后改标签不篡改提交历史。 |

## 4. 非功能要件（非機能要求）

测试门禁必须记录负载、数据集、硬件规模和SLO签字状态；待合意目标不可冒称已通过。

| ID | 类别 | 名称 | 使用条件 | 目标与约束 | 验证方法 |
|---|---|---|---|---|---|
| NFR-AVA-001 | 可用性 | 生产可用率 | E1至少3故障域、正常维护口径按SLA另行约定 | 月度 S3 API 可用率≥99.95%（草案） | MEASURE |
| NFR-AVA-002 | 可用性 | 确认写入持久性 | 任意单节点/单磁盘故障及配置允许的故障数量范围内 | 已返回2xx的对象不得丢失，损坏可自动检出 | CHAOS |
| NFR-AVA-003 | 可用性 | 站点恢复 | 异地主备配置已启用、依赖服务可用 | 生产RTO≤60分钟；异步跨站RPO≤15分钟（草案） | DRILL |
| NFR-AVA-004 | 可用性 | 同站数据一致性 | 针对primary、ACK后单键与LIST可见性 | ACK后GET/HEAD/LIST强一致；跨站异步复制单独声明 | TEST |
| NFR-AVA-005 | 可用性 | WORM恢复安全 | 磁盘失效、节点回滚、时钟漂移 | 保留版本在保留期限内不得通过应用API提前删除 | CHAOS |
| NFR-AVA-006 | 可用性 | 恢复演练周期 | 生产变更与季度计划 | 季度DR演练；每月备份恢复抽样验证 | INSPECT |
| NFR-PER-001 | 性能・拡張性 | S3对象延迟 | B1基线：6节点×16vCPU×64GiB×NVMe×25GbE，3故障域，1MiB对象，300并发 | GET p95≤150ms；PUT p95≤300ms（不含公网） | BENCH |
| NFR-PER-002 | 性能・拡張性 | 数据吞吐 | B1基线，100MiB顺序对象流、预热后连续运行30分钟 | 集群聚合读取吞吐≥1GiB/s，具体由硬件验收确认 | BENCH |
| NFR-PER-003 | 性能・拡張性 | 存储规模 | E1设计容量目标，部署规模/账本成本另行合意 | ≥1亿对象、≥1PiB逻辑命名空间可寻址（压力验证可分层） | BENCH |
| NFR-PER-004 | 性能・拡張性 | 弹性扩容 | B1基线扩容1节点，无计划全站停机 | 扩容期间原API保持可用，数据完整，性能下降范围形成报告 | DRILL |
| NFR-PER-005 | 性能・拡張性 | Git媒体规模 | 单仓库10万LFS对象、单对象≥20GiB | 可上传/下载/恢复/索引且不出现O(size)内存增长 | BENCH |
| NFR-PER-006 | 性能・拡張性 | 并发公平性 | 3租户同时进行S3大对象上传、下载与迁移 | 限流生效，任一租户无法持续耗尽全局带宽/IOPS | BENCH |
| NFR-OPS-001 | 運用・保守性 | 指标覆盖 | 生产运行中的API、节点、KMS、复制、Git网关 | 请求、错误、延迟、容量、校验、排队、滞后均有指标 | INSPECT |
| NFR-OPS-002 | 運用・保守性 | 部署和回滚 | 指定的K8s/VM环境 | 声明式安装、滚动升级、版本回退操作步骤经演练通过 | DRILL |
| NFR-OPS-003 | 運用・保守性 | 数据修复透明 | 任何自愈或恢复操作 | 有操作日志、校验报告、版本证据，不允许静默覆盖 | TEST |
| NFR-OPS-004 | 運用・保守性 | 代码质量 | 核心S3、Git LFS、元数据、授权、迁移代码 | 自动化测试覆盖率目标≥90%；关键路径100%分支用例设计 | CI |
| NFR-OPS-005 | 運用・保守性 | 版本升级兼容 | Rolling upgrade N/N-1服务混部 | API、数据格式与元数据迁移可回退或前置备份校验 | DRILL |
| NFR-OPS-006 | 運用・保守性 | 故障响应 | Sev1/Sev2事件定义合意后 | Sev1自动告警≤5min，操作手册有明确责任人和升级路径 | TEST |
| NFR-MIG-001 | 移行性 | 迁移数据准确度 | ≥100万对象，含非当前版本、标签、锁定对象 | 对象SHA-256及字节长度 100%一致；元数据差异全部列明 | BENCH |
| NFR-MIG-002 | 移行性 | 迁移中断恢复 | 随机kill迁移进程和目标后端5xx | 重启后接续、不出现漏项；无法迁移对象附异常报告 | CHAOS |
| NFR-MIG-003 | 移行性 | 迁移回滚 | 源站保留至独立验证和审批完成 | 失败切换可逆且不会覆盖源站已确认写入 | DRILL |
| NFR-MIG-004 | 移行性 | 外部兼容性 | AWS CLI、aws-sdk-js v3、boto3、Go/Rust S3 SDK、git-lfs | 宣称兼容功能通过协议回归测试≥99%；0个阻断级差异 | TEST |
| NFR-SEC-001 | セキュリティ | 权限拒绝 | 任意匿名、过期、跨租户、撤销凭据 | 敏感资源默认拒绝；跨租户访问成功数=0 | PENTEST |
| NFR-SEC-002 | セキュリティ | 加密 | 生产外部/东西向连接、静态数据 | TLS1.2+；静态强加密并依赖受控KMS；禁用明文后退 | INSPECT |
| NFR-SEC-003 | セキュリティ | 凭据处理 | CI/CD、日志、控制台、事故现场 | 不能输出密钥明文；密钥轮换和吊销符合审计要求 | PENTEST |
| NFR-SEC-004 | セキュリティ | 漏洞要求 | 正式版本发布前及修复SLAs | 0个已知未接受的Critical漏洞；高危经审批例外 | SCAN |
| NFR-SEC-005 | セキュリティ | 审计保留 | 关键安全日志含Object Lock/KMS/Git操作 | 审计事件可关联且在外部不可变存储受控保留≥365天（草案） | INSPECT |
| NFR-SEC-006 | セキュリティ | 供应链验证 | 正式发行包、容器及三方依赖 | SBOM齐全、构建物签名、授权许可证扫描完成 | SCAN |
| NFR-ENV-001 | システム環境・エコロジー | 支持环境 | Linux amd64/arm64、容器、K8s；Windows为CLI/客户端 | 支持矩阵中的环境可重复部署且升级回滚通过 | TEST |
| NFR-ENV-002 | システム環境・エコロジー | 资源效率 | 控制面中等负载（B1），监控处理器与内存 | 资源预算以性能基线测试建立，不以未验证的绝对值承诺 | BENCH |
| NFR-ENV-003 | システム環境・エコロジー | 离线可维护性 | 企业内网环境 | 无公网时安装镜像、文档、许可证/密钥方案可正常运作 | DRILL |
| NFR-ENV-004 | システム環境・エコロジー | 地域节能策略 | 冷热层存储/生命周期需要遵守WORM及数据驻留 | 可呈现资源耗用及预估迁移成本，不因优化误删受保护版本 | INSPECT |
| NFR-GG-001 | VER | 100TB规模恢复一致性 | 100TB逻辑媒体仓库、跨存储迁移、从历史任意Commit恢复 | 0块SHA不一致；不丢已ACK提交 | ST-E2E + content checksum |
| NFR-GG-002 | VER | 版本图可追溯 | 1000万Revision关联不同Commit/Branch/Tag | 全历史可恢复；无悬空边和循环 | ST-graph and restore |
| NFR-GG-003 | PER | 块级增量传输收益 | 指定1GiB构造数据前部插入1KiB | 传输实际缺失Chunk；实验报告字节比及固定分块对照，不固定跨格式承诺 | ST-load profile |
| NFR-GG-004 | PER | 大媒体常数级内存 | 20GiB原件上传/Range/下载及resume | 网关单请求峰值堆内存≤约定阈值且不随文件大小线性增长 | ST-RSS traces |
| NFR-GG-005 | PER | 资产历史查询 | 千万级Revision样本；热缓存与冷查询 | 基线下p95阈值待SLO合意、提供原始分布 | ST-benchmark |
| NFR-GG-006 | REL | 提交与内容强一致 | Ref CAS、元数据Leader failover、Chunk落盘断电 | ACK后内容可还原；缺块不发布新Ref | ST-chaos |
| NFR-GG-007 | REL | 离线冲突零静默丢失 | 两个工作区同一大文件并发离线修改 | 无未提示自动覆盖，存在可审计决议 | ST-conflict |
| NFR-GG-008 | SEC | 媒体插件沙盒边界 | 解析畸形文件、逃逸试探、过载媒体Worker | 核心存储无密钥泄露和代码执行；资源耗尽受限 | ST-security |
| NFR-GG-009 | SEC | 细粒度撤权 | 已签发媒体播放、Range和预览令牌后撤权 | 撤权传播边界经约定；强制访问点不能绕过授权 | ST-auth |
| NFR-GG-010 | MAI | 格式适配器能力清单 | 新增/禁用/升级媒体解析与差异插件 | 未支持能力明确降级；不破坏既存Manifest/Revision | ST-upgrade |
| NFR-GG-011 | MAI | 供应链许可证合规 | 所有Rust crate、JS包、二进制Worker构建 | SBOM覆盖全部发布产物；引入强Copyleft/非自由组件需阻断或批准隔离 | ST-license audit |
| NFR-GG-012 | POR | 数据格式可移植 | 原生Commit/Manifest/Chunk导出与重建 | 不同存储后端/系统版本离线校验可恢复完整媒体 | ST-portability |

## 5. 主要业务规则

| ID | 内容 |
|---|---|
| BR-GG-01 | GitGit Commit/AssetRevision/Manifest/Chunk/S3 logical version/Provider VersionId 为独立标识；不得互换。 |
| BR-GG-02 | GitGit自研版本图与Ref是权威；MinIOS3元数据是内容可靠性与物理存储权威。 |
| BR-GG-03 | Ref CAS成功之前必须得到全部新Chunk已持久化的Receipt；禁止先发布再异步补块。 |
| BR-GG-04 | 原始媒体字节不可变；差异、预览、转码、代理视频只能产生独立派生对象。 |
| BR-GG-05 | 自动合并仅限适配器明确证明安全的媒体格式；其余必须产生人工冲突。 |
| BR-GG-06 | 同一字节可能多Asset/Branch引用；Chunk GC需要全图可达性、保留策略及再验证。 |
| BR-GG-07 | 断点续传和Chunk去重不可作为跨租户内容存在性侧信道。 |
| BR-GG-08 | S3网关写入与GitGit Commit相互独立，S3 VersionId不改变Commit身份。 |
| BR-GG-09 | 格式Adapter升级不改历史ContentManifest和Commit；无法解析时仍可按字节恢复。 |
| BR-GG-10 | 标准Git及LFS兼容仅通过可拔插Adapter提供；停用不影响GitGit原生业务。 |

## 6. 数据与接口要件

GitGit `Repository → Commit DAG → TreeEntry → AssetRevision → ContentManifest → ChunkRef`；元数据状态机保留强主键与约束。GitGit事务Outbox及受信Receipt作为跨服务确认边界。

原生API：`/v1/repositories`, `/v1/assets/sessions`, `/v1/assets/chunks`, `/v1/assets/manifests`, `/v1/commits/prepare`, `/v1/commits/confirm`, `/v1/refs/{ref}`, `/v1/assets/diff`, `/v1/checkout`，详见契约草案。

## 7. 外部接口与互换要件

MinIOS3仍保持S3 REST/SigV4、Bucket/Key版本化、Multipart、ObjectLock、联邦后端迁移及审计功能，遵守对外兼容矩阵。Git LFS Batch/Lock属于E1可选兼容入口。

## 8. 移行・運用・監査・制約

所有媒体解码Worker须隔离；生产使用依赖须经过许可证/SBOM审核；所有S3迁移/Native扩缩容不得破坏任何受保留GitGit Commit可恢复性。

## 9. 验收与质量门禁

- E0：原生提交→大媒体增量→修改→比较→Checkout→恢复的链路必须执行真实自动化测试；不得以旧Git LFS测试代替。
- E1：跨节点/跨AZ故障与WORM合规、高可用、GitGit锁/Review和CAS安全回收必须实际验证。
- E2：媒体适配器有格式支持矩阵和golden样本，无法安全比较或合并须给出可读的退化。
- 设计覆盖不等于实现覆盖；全部测试结果默认为 `Not Run`。

## 10. 变更履历与未决

| 变更集 | 内容 | 状态 |
|---|---|---|
| CR-GG-20261026 | GitGit成为第一方Rust版本引擎；原LFS/外部Git路径降为兼容；新增54 FR、12 NFR、10 UC | 待业务Owner签署 |

GitGit具体代码接口、性能SLO、支持媒体格式级别、S3合规适用范围和商业发布许可均属待审事项。
