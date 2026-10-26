# IMPL-MINIOS3-001 — Rust製造・実装工程规范（非实现代码）

## 1. Cargo workspace 划分（设计目标）
| 分组 | Crates | 责任 |
|---|---|---|
| `gitgit/` | domain, graph, ref-store, workspace, collaboration, commit-coordinator | 自研版本权威 |
| `minios3/` | cas, manifest, chunk-store, metadata-raft, native-ec, s3-gateway, federation, migration | 自研内容与S3存储 |
| `shared/` | ids, protocol, audit, crypto, policy, telemetry | 无循环依赖的共通设施 |
| `apps/` | gitgitd, minios3d, admin-cli, media-worker | 部署进程 |
| `tests/` | contract, fault-injection, compatibility, integration-fixtures | 自动化检查 |

## 2. 依赖原则
核心逻辑Rust；仅在严格需要时接入OpenRaft/redb/OpenDAL/reed-solomon/Tokio/Axum/Tonic；SDK兼容工作由独立测试套件核验。媒体Worker在隔离进程执行不受信解码；FFmpeg非默认核心静态依赖，发行需额外许可证/专利审查。

## 3. 代码质量门禁
- Rustfmt/clippy/测试/依赖漏洞+许可证扫描在CI强制；高风险逻辑必须包含property tests和fault tests。
- 代码修改按`先设计→代码→UT→IT回归`顺序同步文档。编码层面以`Result<_, Error>`显式错误，不允许生产路径unwrap造成崩溃；网络输入限制长度/深度/分配。
- 性能评测冻结Rust版本/MSRV、CPU/磁盘/网络/样本分布；不得把本地mock跑分冒充企业级指标。
- 安全操作使用租户/Principal/AssetId等newtype强类型，引用只能在授权范围内解析，常量时间比较密钥摘要。

## 4. 原子Ref CAS实现顺序
1. 先实现确定性Commit/Tree编码及ID golden vectors。
2. 再实现Local RefStore + single-writer事务 + Outbox。
3. 再以Mock ReceiptVerifier实现正反例；随后接入真实MinIOS3 receipt服务。
4. 最后上线并发、Crash-recovery、GC争用及分布式Raft变更测试。

## 5. 测试构建和产物
各Crate必须公开测试模块引用、构建命令、测试数据许可证、覆盖率定义；发布产物须签名、SBOM、源码版本/配置摘要和变更记录。**当前交付不包括可编译服务，不得写作“实现完成”。**
