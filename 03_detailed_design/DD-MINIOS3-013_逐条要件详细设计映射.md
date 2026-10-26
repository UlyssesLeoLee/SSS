# DD-MINIOS3-013 — 全部功能与非功能要件逐项详细设计映射

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 输入基线：158 FR + 44 NFR；所有要件ID原封保留。静态追踪不代表实现或测试通过。

## 1. 映射规则

每项设计落点记录：原始描述与验收、BD章节、DD编号、Rust责任组件、UT/IT/ST case ID。若设计仍依赖原需求CR或Git接口D-01，则对应测试必须进入Blocked门禁而不能被误标Pass。

## CORE 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-CORE-001 | **独立部署**：断开公网后，既有用户仍可完成鉴权、PUT/GET/LIST、Git LFS兼容 读写和管理。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-001 / IT-FR-CORE-001 / ST-FR-CORE-001 |
| FR-CORE-002 | **Native 与 Federation 运行模式**：两种部署模式均通过 S3 核心兼容测试；模式切换不得改变现有对象内容。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-002 / IT-FR-CORE-002 / ST-FR-CORE-002 |
| FR-CORE-003 | **租户层级**：租户 A 凭据无法枚举或读取租户 B 的任何资源；隔离自动测试100%通过。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-003 / IT-FR-CORE-003 / ST-FR-CORE-003 |
| FR-CORE-004 | **租户资源计量和配额**：超出硬配额返回明确错误，管理员可追踪来源；计量误差可核对。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-004 / IT-FR-CORE-004 / ST-FR-CORE-004 |
| FR-CORE-005 | **存储命名空间和资源映射**：迁移后使用原端点、原 Bucket/Key 可以正确读取原版本。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-005 / IT-FR-CORE-005 / ST-FR-CORE-005 |
| FR-CORE-006 | **配置版本与回滚**：错误路由配置可恢复上一生效版本，不改变已确认对象。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-006 / IT-FR-CORE-006 / ST-FR-CORE-006 |
| FR-CORE-007 | **多区域和数据驻留策略**：禁止区域间转移时，对应迁移/复制任务无法启动并写入审计记录。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-007 / IT-FR-CORE-007 / ST-FR-CORE-007 |
| FR-CORE-008 | **能力发现接口**：API和UI可取得对应版本的能力清单；不支持功能明确返回非兼容错误。 | DD-001, DD-002, DD-004, DD-010, DD-011 | catalog/metadata | UT-FR-CORE-008 / IT-FR-CORE-008 / ST-FR-CORE-008 |

## S3 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-S3-001 | **Bucket 生命周期**：创建、列举、空桶删除、权限错误及地域配置测试通过。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-001 / IT-FR-S3-001 / ST-FR-S3-001 |
| FR-S3-002 | **标准对象读写**：主流 S3 SDK 的上传、下载、覆盖、条件请求符合兼容基线。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-002 / IT-FR-S3-002 / ST-FR-S3-002 |
| FR-S3-003 | **对象列举**：千万级键空间分页不漏不重；与提交确认的写入符合指定一致性。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-003 / IT-FR-S3-003 / ST-FR-S3-003 |
| FR-S3-004 | **分段上传**：分段乱序、断线续传、重复Complete、Abort及并发重试通过。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-004 / IT-FR-S3-004 / ST-FR-S3-004 |
| FR-S3-005 | **版本控制**：覆盖与删除后，历史版本可依授权恢复，版本标识稳定且唯一。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-005 / IT-FR-S3-005 / ST-FR-S3-005 |
| FR-S3-006 | **签名鉴权**：官方AWS SDK认证场景及坏签名、过期时间戳均有正确响应。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-006 / IT-FR-S3-006 / ST-FR-S3-006 |
| FR-S3-007 | **临时预签名**：过期、越权、撤销后无法访问；范围约束与审计关联正确。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-007 / IT-FR-S3-007 / ST-FR-S3-007 |
| FR-S3-008 | **HTTP Range 与流式传输**：对≥20GiB对象进行Range读取，不整体加载内存，返回正确206与校验。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-008 / IT-FR-S3-008 / ST-FR-S3-008 |
| FR-S3-009 | **对象属性及标签**：元数据/标签在复制迁移后按策略完整保留。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-009 / IT-FR-S3-009 / ST-FR-S3-009 |
| FR-S3-010 | **分层策略**：默认拒绝、多租户隔离、策略冲突和SDK常见操作矩阵通过。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-010 / IT-FR-S3-010 / ST-FR-S3-010 |
| FR-S3-011 | **生命周期管理**：TTL到期任务正确执行；被保留/保护的版本不被误删。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-011 / IT-FR-S3-011 / ST-FR-S3-011 |
| FR-S3-012 | **Object Lock**：WORM对覆盖、版本删除、撤销保留、时钟回拨等攻击测试拒绝；独立审计完成。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-012 / IT-FR-S3-012 / ST-FR-S3-012 |
| FR-S3-013 | **事件通知**：消费者重复收到事件也能幂等处理；失败可重放。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-013 / IT-FR-S3-013 / ST-FR-S3-013 |
| FR-S3-014 | **SSE/KMS兼容**：使用配置KMS读写正常，密钥拒绝/轮换行为受测。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-014 / IT-FR-S3-014 / ST-FR-S3-014 |
| FR-S3-015 | **Object Checksum**：篡改块校验失败；迁移前后SHA-256一致。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-015 / IT-FR-S3-015 / ST-FR-S3-015 |
| FR-S3-016 | **S3 Batch 操作**：10万对象批量任务中断重启不丢状态、无重复破坏性变更。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-016 / IT-FR-S3-016 / ST-FR-S3-016 |
| FR-S3-017 | **S3 兼容性清单**：发布版本附机器可读兼容清单，并对宣称支持的API自动回归。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-017 / IT-FR-S3-017 / ST-FR-S3-017 |
| FR-S3-018 | **虚拟主机与路径风格**：两种地址形式在合法桶名、特殊字符、代理TLS下通过。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-018 / IT-FR-S3-018 / ST-FR-S3-018 |
| FR-S3-019 | **对象幂等与并发条件**：重试不会生成损坏版本；并发相同键操作呈现可解释顺序。 | DD-003, DD-004, DD-008, DD-012 | protocol-s3/catalog | UT-FR-S3-019 / IT-FR-S3-019 / ST-FR-S3-019 |

## GIT 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-GIT-001 | **Git LFS兼容 Batch API**：git lfs push/fetch/pull 在不修改Git客户端的情况下完成。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-001 / IT-FR-GIT-001 / ST-FR-GIT-001 |
| FR-GIT-002 | **Git LFS兼容 Locking API**：用户A锁定路径时，用户B的受保护push被拒绝。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-002 / IT-FR-GIT-002 / ST-FR-GIT-002 |
| FR-GIT-003 | **标准Git独立性**：Git仓库迁走LFS端点后，纯文本历史仍由标准Git正常读取。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-003 / IT-FR-GIT-003 / ST-FR-GIT-003 |
| FR-GIT-004 | **内容寻址**：错误size或SHA-256被拒绝；已有OID内容不可替换。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-004 / IT-FR-GIT-004 / ST-FR-GIT-004 |
| FR-GIT-005 | **引用完整性检查**：缺失LFS兼容OID的push被阻止，不能产生成功但不可checkout的提交。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-005 / IT-FR-GIT-005 / ST-FR-GIT-005 |
| FR-GIT-006 | **版本关联索引**：任一commit能定位所有必需媒体OID及存储位置和完整性校验。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-006 / IT-FR-GIT-006 / ST-FR-GIT-006 |
| FR-GIT-007 | **Git分支/Tag与媒体历史**：切换分支/标签时解析出该版本对应媒体，未创建多余新版本。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-007 / IT-FR-GIT-007 / ST-FR-GIT-007 |
| FR-GIT-008 | **Git身份继承**：撤销仓库权限后，新的LFS请求不能通过。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-008 / IT-FR-GIT-008 / ST-FR-GIT-008 |
| FR-GIT-009 | **Git事件接入**：重复消息不重复引用计数；异常事件可修复重放。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-009 / IT-FR-GIT-009 / ST-FR-GIT-009 |
| FR-GIT-010 | **媒体垃圾回收安全**：删除一个分支不会删除被另一分支/Tag/法务锁引用的OID。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-010 / IT-FR-GIT-010 / ST-FR-GIT-010 |
| FR-GIT-011 | **离线和部分克隆**：开发者无需下载完整媒体库即可检出所需工作集。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-011 / IT-FR-GIT-011 / ST-FR-GIT-011 |
| FR-GIT-012 | **仓库配额和限流**：超配额上传在写入前可预测拒绝，已存在历史不被误删。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-012 / IT-FR-GIT-012 / ST-FR-GIT-012 |
| FR-GIT-013 | **提交一致性补偿**：任意步骤断电后，恢复任务识别孤儿OID/悬空引用并给出处理结果。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-013 / IT-FR-GIT-013 / ST-FR-GIT-013 |
| FR-GIT-014 | **现有Git迁移**：不改commit SHA即可迁移对象，双端结果一致。 | DD-006, DD-004, DD-008, DD-012 | protocol-lfs/git-integration | UT-FR-GIT-014 / IT-FR-GIT-014 / ST-FR-GIT-014 |

## MEDIA 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-MEDIA-001 | **多媒体资产分类**：至少PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP完成端到端验证。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-001 / IT-FR-MEDIA-001 / ST-FR-MEDIA-001 |
| FR-MEDIA-002 | **资产元数据**：元数据与Git Commit及OID可以双向检索。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-002 / IT-FR-MEDIA-002 / ST-FR-MEDIA-002 |
| FR-MEDIA-003 | **内容零改写**：下载SHA-256与原上传文件相同。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-003 / IT-FR-MEDIA-003 / ST-FR-MEDIA-003 |
| FR-MEDIA-004 | **异步预览**：预览出错时下载与版本读取不受影响，且有失败原因。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-004 / IT-FR-MEDIA-004 / ST-FR-MEDIA-004 |
| FR-MEDIA-005 | **版本差异辅助**：任意两次Git提交的媒体版本可选择可用的差异方式，能力不足明确提示。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-005 / IT-FR-MEDIA-005 / ST-FR-MEDIA-005 |
| FR-MEDIA-006 | **资产搜索**：指定索引规模下按分页检索，结果遵循权限隔离。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-006 / IT-FR-MEDIA-006 / ST-FR-MEDIA-006 |
| FR-MEDIA-007 | **媒体预览安全**：构造异常媒体文件不能访问宿主机密钥或其他租户对象。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-007 / IT-FR-MEDIA-007 / ST-FR-MEDIA-007 |
| FR-MEDIA-008 | **预览内容与源关联**：源变化触发可识别的重新生成；源内容保持不可变。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-008 / IT-FR-MEDIA-008 / ST-FR-MEDIA-008 |
| FR-MEDIA-009 | **媒体交付**：断网后续传与视频Seek测试通过；缓存不泄漏租户数据。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-009 / IT-FR-MEDIA-009 / ST-FR-MEDIA-009 |
| FR-MEDIA-010 | **许可与溯源**：受限制素材无法被标记为可公开发布。 | DD-009, DD-006, DD-010 | media/preview-worker | UT-FR-MEDIA-010 / IT-FR-MEDIA-010 / ST-FR-MEDIA-010 |

## XCHG 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-XCHG-001 | **S3后端适配**：同一测试数据在至少AWS S3与另一个自建S3实现上完整往返。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-001 / IT-FR-XCHG-001 / ST-FR-XCHG-001 |
| FR-XCHG-002 | **非S3供应商适配**：增补适配器无需更改S3客户端公开接口。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-002 / IT-FR-XCHG-002 / ST-FR-XCHG-002 |
| FR-XCHG-003 | **统一端点**：原有客户端仅改Endpoint/密钥即可访问受支持S3 API。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-003 / IT-FR-XCHG-003 / ST-FR-XCHG-003 |
| FR-XCHG-004 | **后端能力矩阵**：要求WORM而后端不满足时禁止部署，不允许虚假成功。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-004 / IT-FR-XCHG-004 / ST-FR-XCHG-004 |
| FR-XCHG-005 | **存储策略与放置**：Dry-run输出每条规则的匹配原因和成本估计。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-005 / IT-FR-XCHG-005 / ST-FR-XCHG-005 |
| FR-XCHG-006 | **迁移作业**：百万对象迁移中断后恢复，不漏文件；核验摘要与元数据。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-006 / IT-FR-XCHG-006 / ST-FR-XCHG-006 |
| FR-XCHG-007 | **迁移预演**：存在不兼容Object Lock时必须报阻断项。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-007 / IT-FR-XCHG-007 / ST-FR-XCHG-007 |
| FR-XCHG-008 | **无损切换**：生产演练迁移失败可回退到源端，用户已确认写入不丢失。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-008 / IT-FR-XCHG-008 / ST-FR-XCHG-008 |
| FR-XCHG-009 | **双端校验**：对人为损坏和缺失分别准确告警。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-009 / IT-FR-XCHG-009 / ST-FR-XCHG-009 |
| FR-XCHG-010 | **连续复制**：乱序和网络分区不产生静默覆盖；复制滞后指标可见。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-010 / IT-FR-XCHG-010 / ST-FR-XCHG-010 |
| FR-XCHG-011 | **冲突控制**：并发写同键情况下不允许最后写入静默掩盖冲突。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-011 / IT-FR-XCHG-011 / ST-FR-XCHG-011 |
| FR-XCHG-012 | **读取故障转移**：故障转移返回一致性状态；未同步数据不得误报存在。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-012 / IT-FR-XCHG-012 / ST-FR-XCHG-012 |
| FR-XCHG-013 | **后端出口开放**：第三方工具可从导出的数据重建完整键空间及版本关系。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-013 / IT-FR-XCHG-013 / ST-FR-XCHG-013 |
| FR-XCHG-014 | **成本与计量**：成本报表能与供应商账单进行差额核对。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-014 / IT-FR-XCHG-014 / ST-FR-XCHG-014 |
| FR-XCHG-015 | **底层供应商异常隔离**：一个后端故障不会拖垮其它桶的正常请求。 | DD-007, DD-004, DD-008, DD-011 | storage-s3/migration | UT-FR-XCHG-015 / IT-FR-XCHG-015 / ST-FR-XCHG-015 |

## NATIVE 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-NATIVE-001 | **多节点横向扩展**：扩容演练不需要停掉全部S3服务。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-001 / IT-FR-NATIVE-001 / ST-FR-NATIVE-001 |
| FR-NATIVE-002 | **纠删码/数据保护**：配置容错范围内故障可读取完整数据，超出范围进入安全只读/错误状态。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-002 / IT-FR-NATIVE-002 / ST-FR-NATIVE-002 |
| FR-NATIVE-003 | **故障域意识**：放置审计证明碎片满足故障域策略。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-003 / IT-FR-NATIVE-003 / ST-FR-NATIVE-003 |
| FR-NATIVE-004 | **节点自愈与巡检**：混合故障注入后数据摘要不变，进度和剩余风险可查询。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-004 / IT-FR-NATIVE-004 / ST-FR-NATIVE-004 |
| FR-NATIVE-005 | **存储一致性语义**：ACK后GET/HEAD/LIST（在规定作用域）立即可见，不出现半对象。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-005 / IT-FR-NATIVE-005 / ST-FR-NATIVE-005 |
| FR-NATIVE-006 | **数据与控制面故障隔离**：单点注入与恢复试验通过。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-006 / IT-FR-NATIVE-006 / ST-FR-NATIVE-006 |
| FR-NATIVE-007 | **容量水位保护**：容量接近上限能预测并安全拒绝写入。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-007 / IT-FR-NATIVE-007 / ST-FR-NATIVE-007 |
| FR-NATIVE-008 | **数据损坏检测**：注入存储位错误后检测到并在冗余足够时自愈。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-008 / IT-FR-NATIVE-008 / ST-FR-NATIVE-008 |
| FR-NATIVE-009 | **故障演练接口**：每个发布候选版本执行故障注入测试。 | DD-005, DD-004, DD-011 | storage-native/metadata | UT-FR-NATIVE-009 / IT-FR-NATIVE-009 / ST-FR-NATIVE-009 |

## SEC 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-SEC-001 | **SSO与身份联邦**：SSO登录/登出/禁用账号后会话生效性经测试。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-001 / IT-FR-SEC-001 / ST-FR-SEC-001 |
| FR-SEC-002 | **IAM/RBAC/ABAC**：尝试越权访问、横向提权和策略覆盖均失败。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-002 / IT-FR-SEC-002 / ST-FR-SEC-002 |
| FR-SEC-003 | **短期凭据**：过期Token和已撤销AccessKey被拒绝并审计。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-003 / IT-FR-SEC-003 / ST-FR-SEC-003 |
| FR-SEC-004 | **传输加密**：未加密入口默认关闭；TLS配置可扫描验证。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-004 / IT-FR-SEC-004 / ST-FR-SEC-004 |
| FR-SEC-005 | **静态加密与密钥管理**：KMS故障不会造成明文回退，轮换后的旧数据可访问。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-005 / IT-FR-SEC-005 / ST-FR-SEC-005 |
| FR-SEC-006 | **审计追踪**：追踪一个Git Commit至对应OID和S3 Version与访问日志。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-006 / IT-FR-SEC-006 / ST-FR-SEC-006 |
| FR-SEC-007 | **不可篡改审计**：系统管理员不能通过普通运维权限覆盖已封存审计记录。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-007 / IT-FR-SEC-007 / ST-FR-SEC-007 |
| FR-SEC-008 | **恶意软件与内容控制**：恶意媒体可隔离并可追查，原始SHA-256和隔离动作有证据。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-008 / IT-FR-SEC-008 / ST-FR-SEC-008 |
| FR-SEC-009 | **管理面防护**：删除存储池、强制解锁、迁移切换触发审批规则。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-009 / IT-FR-SEC-009 / ST-FR-SEC-009 |
| FR-SEC-010 | **安全加固与供应链**：关键漏洞阻止正式发布，SBOM可下载。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-010 / IT-FR-SEC-010 / ST-FR-SEC-010 |
| FR-SEC-011 | **合法持有与数据隔离**：租户删除前检测WORM和Git活跃引用，依法合规执行。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-011 / IT-FR-SEC-011 / ST-FR-SEC-011 |
| FR-SEC-012 | **秘密管理**：数据导出和日志不包含明文AccessKey/SecretKey。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-012 / IT-FR-SEC-012 / ST-FR-SEC-012 |
| FR-SEC-013 | **勒索防护**：批量误删后可以按保留策略恢复；模拟攻击产生报警。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-013 / IT-FR-SEC-013 / ST-FR-SEC-013 |
| FR-SEC-014 | **合规声明边界**：发布材料包含适用范围、例外、证据和未认证声明。 | DD-008, DD-002, DD-010 | authz/security/audit | UT-FR-SEC-014 / IT-FR-SEC-014 / ST-FR-SEC-014 |

## OPS 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| FR-OPS-001 | **Web管理控制台**：任务列表和对象详情三步内可进入，角色不可见不属于自己的资源。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-001 / IT-FR-OPS-001 / ST-FR-OPS-001 |
| FR-OPS-002 | **版本关系可视化**：定位一项历史资源无需理解后端私有路径。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-002 / IT-FR-OPS-002 / ST-FR-OPS-002 |
| FR-OPS-003 | **迁移与复制工作台**：操作可审计；危险动作有影响预览。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-003 / IT-FR-OPS-003 / ST-FR-OPS-003 |
| FR-OPS-004 | **媒体浏览器**：图像、视频、3D预览异常不阻断下载。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-004 / IT-FR-OPS-004 / ST-FR-OPS-004 |
| FR-OPS-005 | **多语言**：语言切换不改变权限/对象数据；日语信息自然一致。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-005 / IT-FR-OPS-005 / ST-FR-OPS-005 |
| FR-OPS-006 | **无障碍与认知负荷**：键盘完成桶创建、恢复版本和权限查询，核心流程可用性评审通过。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-006 / IT-FR-OPS-006 / ST-FR-OPS-006 |
| FR-OPS-007 | **CLI与配置即代码**：操作与Web实现相同权限与审计策略。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-007 / IT-FR-OPS-007 / ST-FR-OPS-007 |
| FR-OPS-008 | **API文档与SDK示例**：新开发者按文档完成上传、Git LFS兼容和迁移样例。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-008 / IT-FR-OPS-008 / ST-FR-OPS-008 |
| FR-OPS-009 | **审计查询与报告**：报告可重现追查一次管理员动作与Git修改。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-009 / IT-FR-OPS-009 / ST-FR-OPS-009 |
| FR-OPS-010 | **监控与告警**：Grafana/兼容仪表板可读取关键指标并按阈值触发。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-010 / IT-FR-OPS-010 / ST-FR-OPS-010 |
| FR-OPS-011 | **零停机滚动升级**：在允许的单节点轮替中读写持续；失败能按预案恢复。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-011 / IT-FR-OPS-011 / ST-FR-OPS-011 |
| FR-OPS-012 | **备份与恢复**：仅从备份与对象数据可复原Git映射和用户可读对象。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-012 / IT-FR-OPS-012 / ST-FR-OPS-012 |
| FR-OPS-013 | **异常与根因信息**：S3错误+后台trace可串联到具体后端与操作。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-013 / IT-FR-OPS-013 / ST-FR-OPS-013 |
| FR-OPS-014 | **运维 runbook**：演练人员不依赖开发者介入完成预定操作。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-014 / IT-FR-OPS-014 / ST-FR-OPS-014 |
| FR-OPS-015 | **服务水平报告**：月报按原始监控数据复算一致。 | DD-010, DD-011, DD-012 | admin-contract/observability | UT-FR-OPS-015 / IT-FR-OPS-015 / ST-FR-OPS-015 |

## NFR 要件

| 要件ID | 原标题/验收条件 | 详细设计 | 组件 | 测试ID |
|---|---|---|---|---|
| NFR-AVA-001 | **生产可用率**：月度 S3 API 可用率≥99.95%（草案） | DD-004, DD-005, DD-007, DD-011 | DD-004/DD-005 | UT-NFR-AVA-001 / — / ST-NFR-AVA-001 |
| NFR-AVA-002 | **确认写入持久性**：已返回2xx的对象不得丢失，损坏可自动检出 | DD-004, DD-005, DD-007, DD-011 | DD-004/DD-005 | UT-NFR-AVA-002 / — / ST-NFR-AVA-002 |
| NFR-AVA-003 | **站点恢复**：生产RTO≤60分钟；异步跨站RPO≤15分钟（草案） | DD-004, DD-005, DD-007, DD-011 | DD-004/DD-005 | UT-NFR-AVA-003 / — / ST-NFR-AVA-003 |
| NFR-AVA-004 | **同站数据一致性**：ACK后GET/HEAD/LIST强一致；跨站异步复制单独声明 | DD-004, DD-005, DD-007, DD-011 | DD-004/DD-005 | UT-NFR-AVA-004 / — / ST-NFR-AVA-004 |
| NFR-AVA-005 | **WORM恢复安全**：保留版本在保留期限内不得通过应用API提前删除 | DD-004, DD-005, DD-007, DD-011 | DD-004/DD-005 | UT-NFR-AVA-005 / — / ST-NFR-AVA-005 |
| NFR-AVA-006 | **恢复演练周期**：季度DR演练；每月备份恢复抽样验证 | DD-004, DD-005, DD-007, DD-011 | DD-004/DD-005 | UT-NFR-AVA-006 / — / ST-NFR-AVA-006 |
| NFR-PER-001 | **S3对象延迟**：GET p95≤150ms；PUT p95≤300ms（不含公网） | DD-002, DD-005, DD-011 | DD-002/DD-005 | — / — / ST-NFR-PER-001 |
| NFR-PER-002 | **数据吞吐**：集群聚合读取吞吐≥1GiB/s，具体由硬件验收确认 | DD-002, DD-005, DD-011 | DD-002/DD-005 | — / — / ST-NFR-PER-002 |
| NFR-PER-003 | **存储规模**：≥1亿对象、≥1PiB逻辑命名空间可寻址（压力验证可分层） | DD-002, DD-005, DD-011 | DD-002/DD-005 | — / — / ST-NFR-PER-003 |
| NFR-PER-004 | **弹性扩容**：扩容期间原API保持可用，数据完整，性能下降范围形成报告 | DD-002, DD-005, DD-011 | DD-002/DD-005 | — / — / ST-NFR-PER-004 |
| NFR-PER-005 | **Git媒体规模**：可上传/下载/恢复/索引且不出现O(size)内存增长 | DD-002, DD-005, DD-011 | DD-002/DD-005 | — / — / ST-NFR-PER-005 |
| NFR-PER-006 | **并发公平性**：限流生效，任一租户无法持续耗尽全局带宽/IOPS | DD-002, DD-005, DD-011 | DD-002/DD-005 | — / — / ST-NFR-PER-006 |
| NFR-OPS-001 | **指标覆盖**：请求、错误、延迟、容量、校验、排队、滞后均有指标 | DD-010, DD-011 | DD-010/DD-011 | — / — / ST-NFR-OPS-001 |
| NFR-OPS-002 | **部署和回滚**：声明式安装、滚动升级、版本回退操作步骤经演练通过 | DD-010, DD-011 | DD-010/DD-011 | — / — / ST-NFR-OPS-002 |
| NFR-OPS-003 | **数据修复透明**：有操作日志、校验报告、版本证据，不允许静默覆盖 | DD-010, DD-011 | DD-010/DD-011 | — / — / ST-NFR-OPS-003 |
| NFR-OPS-004 | **代码质量**：自动化测试覆盖率目标≥90%；关键路径100%分支用例设计 | DD-010, DD-011 | DD-010/DD-011 | — / — / ST-NFR-OPS-004 |
| NFR-OPS-005 | **版本升级兼容**：API、数据格式与元数据迁移可回退或前置备份校验 | DD-010, DD-011 | DD-010/DD-011 | — / — / ST-NFR-OPS-005 |
| NFR-OPS-006 | **故障响应**：Sev1自动告警≤5min，操作手册有明确责任人和升级路径 | DD-010, DD-011 | DD-010/DD-011 | — / — / ST-NFR-OPS-006 |
| NFR-MIG-001 | **迁移数据准确度**：对象SHA-256及字节长度 100%一致；元数据差异全部列明 | DD-007, DD-012 | DD-007/DD-012 | — / IT-NFR-MIG-001 / ST-NFR-MIG-001 |
| NFR-MIG-002 | **迁移中断恢复**：重启后接续、不出现漏项；无法迁移对象附异常报告 | DD-007, DD-012 | DD-007/DD-012 | — / IT-NFR-MIG-002 / ST-NFR-MIG-002 |
| NFR-MIG-003 | **迁移回滚**：失败切换可逆且不会覆盖源站已确认写入 | DD-007, DD-012 | DD-007/DD-012 | — / IT-NFR-MIG-003 / ST-NFR-MIG-003 |
| NFR-MIG-004 | **外部兼容性**：宣称兼容功能通过协议回归测试≥99%；0个阻断级差异 | DD-007, DD-012 | DD-007/DD-012 | — / IT-NFR-MIG-004 / ST-NFR-MIG-004 |
| NFR-SEC-001 | **权限拒绝**：敏感资源默认拒绝；跨租户访问成功数=0 | DD-008, DD-011 | DD-008/DD-011 | UT-NFR-SEC-001 / IT-NFR-SEC-001 / ST-NFR-SEC-001 |
| NFR-SEC-002 | **加密**：TLS1.2+；静态强加密并依赖受控KMS；禁用明文后退 | DD-008, DD-011 | DD-008/DD-011 | UT-NFR-SEC-002 / IT-NFR-SEC-002 / ST-NFR-SEC-002 |
| NFR-SEC-003 | **凭据处理**：不能输出密钥明文；密钥轮换和吊销符合审计要求 | DD-008, DD-011 | DD-008/DD-011 | UT-NFR-SEC-003 / IT-NFR-SEC-003 / ST-NFR-SEC-003 |
| NFR-SEC-004 | **漏洞要求**：0个已知未接受的Critical漏洞；高危经审批例外 | DD-008, DD-011 | DD-008/DD-011 | UT-NFR-SEC-004 / IT-NFR-SEC-004 / ST-NFR-SEC-004 |
| NFR-SEC-005 | **审计保留**：审计事件可关联且在外部不可变存储受控保留≥365天（草案） | DD-008, DD-011 | DD-008/DD-011 | UT-NFR-SEC-005 / IT-NFR-SEC-005 / ST-NFR-SEC-005 |
| NFR-SEC-006 | **供应链验证**：SBOM齐全、构建物签名、授权许可证扫描完成 | DD-008, DD-011 | DD-008/DD-011 | UT-NFR-SEC-006 / IT-NFR-SEC-006 / ST-NFR-SEC-006 |
| NFR-ENV-001 | **支持环境**：支持矩阵中的环境可重复部署且升级回滚通过 | DD-011, DD-010 | DD-011/DD-010 | — / IT-NFR-ENV-001 / ST-NFR-ENV-001 |
| NFR-ENV-002 | **资源效率**：资源预算以性能基线测试建立，不以未验证的绝对值承诺 | DD-011, DD-010 | DD-011/DD-010 | — / IT-NFR-ENV-002 / ST-NFR-ENV-002 |
| NFR-ENV-003 | **离线可维护性**：无公网时安装镜像、文档、许可证/密钥方案可正常运作 | DD-011, DD-010 | DD-011/DD-010 | — / IT-NFR-ENV-003 / ST-NFR-ENV-003 |
| NFR-ENV-004 | **地域节能策略**：可呈现资源耗用及预估迁移成本，不因优化误删受保护版本 | DD-011, DD-010 | DD-011/DD-010 | — / IT-NFR-ENV-004 / ST-NFR-ENV-004 |

## 2. 评审与发布约束

不能依据本表的100%静态映射声明通过企业级验收；需执行UT/IT/ST、保存实际测试日志和缺陷单，并签署本包05_quality中的阻断项。


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
