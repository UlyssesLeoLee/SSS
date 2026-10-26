# OSS-MINIOS3-001 — Rust技术选型、许可证与合规清单

> 本清单为预选，不保证锁定版本及传递依赖已审查；发行以Cargo.lock、完整SBOM和每项实际SPDX信息为准。

| 层级 | 候选 | 用途 | 许可证及风险确认点 |
|---|---|---|---|
| 核心语言/并发 | Rust + Tokio | 自研GitGit/MinIOS3 | Rust工具链分发/依赖遵守对应宽松许可证 |
| API | Axum/Hyper, Tonic/Prost | S3、GitGit Native API、RPC | MIT等宽松许可证，锁定版本复核 |
| 元数据共识 | OpenRaft + redb | Raft状态机与本地KV | 宽松许可；一致性需自证 |
| 联邦存储 | Apache OpenDAL | 外部适配 | Apache-2.0；S3语义由自研网关提供 |
| AWS S3适配 | AWS SDK Rust | 第三方S3后端 | Apache-2.0；兼容矩阵需实测 |
| 内容管理 | FastCDC、BLAKE3、RustCrypto | 分块、哈希、加密 | 核对crate实际SPDX、算法/合规用途 |
| 权限 | Cedar | 策略评估 | Apache-2.0；认证与授权分离 |
| 搜索 | Tantivy | 媒体元数据索引 | MIT；索引不作权威数据源 |
| 前端 | Vue3/TypeScript/Vite/ECharts/Three.js | 控制台及差异预览 | 分别核对MIT/Apache许可和第三方插件 |
| 身份与机密管理 | Keycloak/OpenBao | 外围部署服务 | Apache-2.0/MPL-2.0等；分发时履行通知和源码义务 |
| 视频Worker | 可选FFmpeg | 代理视频/转码 | 依据实际配置LGPL/GPL/nonfree，专利另审 |

## 发布门禁
`cargo deny check licenses`或等价工具、Cargo.lock+NPM lockfile、容器镜像基础层SBOM、NOTICE/源归属清单、GPL/AGPL/LGPL/MPL隔离审查、媒体格式专利风险清单及对外许可证文案。不能仅凭上表声称“所有依赖无条件可闭源商用”。
