# BD-MINIOS3-012 — GitGit 原生多媒体版本管理基本设计

> v2.0 · 设计主权：GitGit Rust Core，不借助外部Git内核

## 一、系统背景 / 業務システムの構成

GitGit 是多媒体版本控制层；MinIOS3 是块级内容和对象存储层。S3统一端点是兼容生态手段，而非资产版本的核心标识。Repository下多个Branch/Tag指向Commit DAG；Commit Tree指向稳定AssetId的Revision；Manifest引用一个或多个不可变Chunk。采用多层权威与受信收据防止跨服务半提交。

## 二、系统模块与Rust工作区

`gitgit-domain`、`gitgit-graph`、`gitgit-ref-store`、`gitgit-commit-coordinator`、`gitgit-workspace`、`gitgit-collaboration`、`gitgit-media-diff`、`gitgit-interop-git`（可选）；MinIOS3 的 `chunk-cas`、`manifest-catalog`、`media-adapter-api`、`native-ec`、`s3-gateway`、`storage-federation`、`gc-coordinator`、`authz`、`audit-outbox`。

## 三、功能架构

1. GitGit版本图存储和修改采用`expected_old_ref_revision`的强一致CAS，Commit先持久化再发布Ref。
2. MinIOS3 Manifest `manifest_id = SHA-256(canonical_manifest_bytes)`；hash使用确定性序列化与版本化算法；Chunk ID必须包括租户加密作用域。
3. 上传返回带 `durable_at`, `placement_epoch`, `policy_epoch`, `manifest_hash`, `fencing_token`的不可转让Receipt；GitGit prepare在提交瞬间再校验。
4. 签名Outbox记录GitGit成功发布事件，异步构建依赖图、搜索、审计镜像；GC必须取得签名可达性水位和物理Retention判断。
5. MediaAdapter按能力表处理inspect/preview/diff/merge；不支持复杂媒体merge时强制冲突，由操作者决议。

## 四、可靠性/安全性要点

引入 `UploadSession`→`ChunksVerified`→`ManifestSealed`→`CommitPrepared`→`RefPublished`→`OutboxReconciled`；失败与重试幂等，禁止`RefPublished`先于`ManifestSealed`。GC采用双阶段Tombstone与再验证；迁移采用阻塞切换watermark。在资产跨项目复用时不泄露哈希存在性与加密域信息。

## 五、图 / 数据 / 画面

逻辑ER见 `../06_diagrams/11_gitgit_media_domain_er.mmd`；写入时序见`../06_diagrams/12_gitgit_native_commit.mmd`；协作/编辑画布支持资产版本树、双轨差异图、Review和依赖DAG，E0首先提供常规列表与双版本对比，3D交互E2。

## 六、外部接口与可验证验收

GitGit Native API及MinIOS3 Content API在 `04_contracts/gitgit_native_openapi.yaml`（示例设计）内定义，LFS可选后置Adapter。`FR-GG-001～020`及`FR-MV-001～034`追踪到`DD-MINIOS3-014～019`和UT/IT/ST设计清单，不宣称已验证实现。
