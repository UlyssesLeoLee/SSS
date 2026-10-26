# GitGit + MinIOS3 多媒体版本管理 — 日本IPA体系设计包 v2.0

> 日期 2026-10-26 ｜状态 Design Draft, Not Implemented, Not Tested

## 产品主张

**GitGit完全自研Rust Commit/Ref/Branch/AssetRevision与多媒体版本管理；MinIOS3完全自研Rust CAS/Manifest/S3 Gateway/Storage Interchange；Git LFS/标准Git是可选兼容接口。**

## 内容目录

- `00_governance/` 产品主权边界、ADR、正式需求修订追踪
- `01_requirements/` IPA结构参照的完整SRS和机器可读的所有 158 FR、44 NFR、22 UC
- `02_basic_design/` Rust基本设计主书+12号GitGit专题及既有存储专题
- `03_detailed_design/` 13个原DD编号继承，增补14~19号Rust原生GitGit/多媒体专题
- `04_contracts/` GitGit Native API OpenAPI/JSON、Manifest Schema、MediaAdapter能力矩阵、S3与可选LFS互操作
- `05_test_design/` UT/IT/ST设计书、详细案例及CSV/JSON、环境和总体计划
- `06_diagrams/` Mermaid ER/时序/状态/架构图源码
- `07_examples/` Rust服务部署配置样例（非可执行部署产物）
- `08_quality/` 对抗审查、静态覆盖与追踪索引
- `MinIOS3_GitGit_IPA_Traceability_v2.0.xlsx` 全需求、逐条测试、变更、风险、审查门禁的工作簿

## 测试设计规模

**UT 186、IT 194、ST 249、总计 629 个设计案例。** 全部 `Not Run`。原版旧案例保留其ID，但涉及Git的语义按v2.0改成原生Core或LFS可选兼容。

## 设计审查与实现准备

- 严格区分GitGit Version / ContentManifest / S3 Logical Version / Provider Physical Version。
- GitGit `Commit/Ref CAS` 只有在MinIOS3签署DurableReceipt且已核验后才发布。
- 无法合并的二进制媒体显式冲突；图片/音频/视频/3D适配器按能力逐步实现。
- 媒体原件不变、派生缓存可再生成；安全GC依完整可达性/保留规则。
- 许可证默认优先MIT/Apache/BSD，实际发行前审查锁定版本的传递依赖/Worker构建。
- 所有图、接口和Rust代码片段为**设计示例，不代表现成程序或测试通过**。

## 审查优先级

首先签署产品边界、GitGit数据库/Ref CAS的持久化方案、Receipt票据不可撤销边界和20GiB端到端PoC；随后才能冻结Rust详细设计并进入工程实现。
