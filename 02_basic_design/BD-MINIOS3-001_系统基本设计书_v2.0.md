# BD-MINIOS3-001 — GitGit + MinIOS3 Rust 系统基本设计书

> v2.0 | 2026-10-26 | 日本IPA结构参照；待评审设计，非已实现系统

## 1. 目标及系统化范围

GitGit+MinIOS3是**多媒体版本管理产品**。GitGit拥有自研Commit/Ref/Branch/Tag/Review/Lock/AssetRevision权威；MinIOS3提供可验证不可变CAS、Chunk/Manifest、Native/联邦S3后端及企业级存储服务。Git LFS/标准Git为互操作Adapter，不是实现前提。

## 2. 业务架构及阶段

| 阶段 | 业务功能 | 成功边界 |
|---|---|---|
| E0 | Repo/Commit/Ref、视频/图片/音频/3D任意字节版本管理、CDC分块、增量传输、原生REST+S3兼容、历史恢复 | Ref CAS + durable receipts + hash restore |
| E1 | 资产依赖、媒体差异、锁/评审、HA分布式原生存储、WORM、安全审计、迁移、可选LFS适配 | 多故障域、GC与授权边界通过真实IT/ST |
| E2 | 音视频/3D深度对比、专用格式插件、Git导入及工作流 | goldens/许可证/格式支持矩阵 |

## 3. Rust服务部署和责任边界

- `gitgit-core`: Rust Commit DAG/Ref CAS/Repo Tree/AssetRevision/Review/Lock/Outbox；拥有GitGit提交结果的最终解释权。
- `minios3-gateway`: Rust 原生媒体API + S3 SigV4网关；流式读写、授权、原生Chunk sessions。
- `minios3-meta`: OpenRaft元数据控制组（E0单组）；存储Manifest/Placement/Receipt/WORM/route epoch，不修改GitGit Ref。
- `minios3-storage`: Rust原生Chunk物理面、4+2纠删码（E1）、Bitrot Scrub/自愈。
- `media-worker`: 隔离沙盒MediaAdapter插件、Image/Video/Audio/3D preview+diff；禁读取服务端存储主密钥。
- `admin-api` + `console`: 管理/任务、审计/监控、租户治理；Browser使用Vue/TypeScript，后端Rust。

## 4. 元数据主权及一致性

| 数据 | 记录权威 | 对外一致性 |
|---|---|---|
| Ref/Commit/Tree/AssetRevision | GitGit Core持久元数据状态机 | CAS线性化发布、不可变历史 |
| ContentManifest/ChunkRef/StorageReceipt | MinIOS3 Meta/Storage | Durable后签署，未满足拒绝 |
| 资产搜索与预览 | 可重建派生索引 | 最终一致，展示watermark |
| S3逻辑版本、锁、物理Version映射 | MinIOS3 Meta | 主站强一致；迁移映射版本保持 |

GitGit与MinIOS3**不承诺跨服务ACID**；使用受信Receipt→GitGit Prepare→Ref CAS→GitGit outbox→确认/幂等对账。未能确认的Upload暂时受保留且通过事务补偿清理，绝不可发布缺Chunk Commit。

## 5. 内容分块和媒体版本生命周期

`File → CDC Chunk(s) → ContentManifest → AssetRevision → TreeEntry → Commit → Ref`。ContentManifest使用稳定编码/版本、全文件SHA-256、每Chunk摘要/Offset/Length、加密域和存储Receipt；AssetRevision持有独立ID与parent revisions。媒体Adapter仅对该不可变内容执行分析、预览和差异。

## 6. 用户核心操作

- 导入：新建UploadSession → 分块/hash → 查询已存在Chunk（受scope限制）→ 缺块上传与校验 → Manifest签名登记。
- 提交：GitGit读取WorkingTree → 校验全部Manifest的Receipt/ACL/策略epoch → Atomically CAS Ref + outbox。
- 差异：比较左右AssetRevision的Manifest与Adapter差异结果，支持图片热区、音频波形、视频时间码、3D mesh/材质；不支持就hash退化。
- 合并：先走可合并的MediaAdapter结构化3-way；未知二进制必须冲突/人工决议，不能直接覆盖。
- 恢复：Commit Tree → AssetRevision → Manifest → Chunk streaming+hash → 工作区，严格保持原始字节。

## 7. 安全与企业治理

所有访问按tenant/repo/ref/asset/revision独立授权；GitGit返回的签名读取凭据不能绕过MinIOS3数据面撤权/retention。WORM/ObjectLock保护具体S3逻辑对象版本，GitGit Ref删除不允许触发受保留Chunk物理删除。审计Outbox事务化记录Ref变更、策略修改、GC及Cutover。

## 8. S3接口/互换/Native可靠性

S3 Gateway支持约定兼容矩阵（SigV4、Multipart、Versioning、ObjectLock、Range、LIST）；GitGit Revision与S3 LogicalVersion/Provider VersionId严格区分。Native使用EC纠删及Raft确认。E1单Bucket默认为单元数据Raft分片，直到跨分片LIST一致性有证据。Fed迁移遵循Dry-run→Copy→Verify→Catchup→WriteFence→Cutover→Monitor，切回旧源之前必须追平新写入。

## 9. 非功能和运行架构

部署E0单机或Podman/Compose与E1 K3s/专有VM可选；服务数据面水平扩展，管理面独立；OpenTelemetry/Prometheus观测；KMS可选OpenBao但许可证另审；GitGit版本面备份须与MinIOS3 Manifest/Chunk/KMS快照互相核对。

## 10. 设计资料/验收追踪

`BD-MINIOS3-012`专门定义GitGit/多媒体架构；`DD-MINIOS3-014～019`给出Rust Trait、数据字典、状态机、协作与媒体Adapter。所有原FR/NFR及新增要件映射至Excel。UT/IT/ST只完成设计，**结果全部Not Run**。设计冻结条件：原生GitGit接口PoC、20GiB流式传输、授权/GC/分布式故障测试、OSS供应链许可证批准。
