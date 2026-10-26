# TP-MINIOS3-001 — GitGit + MinIOS3 全体测试计划 / テスト計画

> v2.0 | 2026-10-26 | **测试设计已编制，未执行**

## 1. 测试基线和执行顺序

以SRS全部 158 FR、44 NFR、22 UC为追踪对象；UT 186，IT 194，ST 249，总计 629 条设计用例。按设计→代码→测试的质量门禁执行；生成文档不代表自动测试通过。

## 2. 阶段门禁

| 阶段 | 输入 | 输出 | 阻断示例 |
|---|---|---|---|
| UT | DD + Rust crate + goldens | assertions, proptest, cov报告 | Commit/Manifest不可变性缺陷、panic、权限判定缺陷 |
| IT | 真实GitGit Rust+MinIOS3+S3 Provider | 协议trace、SHA、恢复证据 | GitGit Ref CAS半提交、Receipt不可信、错误授权 |
| ST | 多用户/真实媒体/故障域/HA部署 | 业务流程、负载、灾备、WORM与审批签收 | 丢已ACK提交、越权读、误GC、不可恢复媒体 |

## 3. 核心主链路覆盖

原生`UploadSession→CDC→ManifestSealed→GitGit Ref CAS→MediaDiff→Merge/Conflict→History Checkout→GC`为E0/E1不可替代业务主链路。Git LFS兼容和传统Git导入仅按原SRS变更记录另外验收，不可代替该主链路。

## 4. 用例结果及覆盖口径

每Case统一 Not Run/Pass/Fail/Blocked/NA，唯一CaseID，保留Rust Build SHA、Test Fixture hash、Runner、ExecutedAt、Evidence URI、Defect ID；仅静态设计映射不计运行覆盖率。

## 5. 退出标准

关键MUST要件全设计有Test ID；真实完成后合格率和Blocker依发布阶段评审，缺少GitGit原生服务、未签署SLO或许可证审核即保持对应ST Blocked。
