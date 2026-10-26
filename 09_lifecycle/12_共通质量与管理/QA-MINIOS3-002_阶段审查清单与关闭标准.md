# QA-MINIOS3-002 — 工程全阶段审查与质量门禁

## 1. 各阶段评审
| 阶段 | 审查点 | 阻断项 | 证据 |
|---|---|---|---|
| 企画 | 多媒体版本目标、技术产权、可行性 | 产品主权模糊 | Charter+PoC计划 |
| 要件定义 | FR/NFR/UC唯一ID、测试可判定 | 无法验收的承诺 | SRS+review record |
| 基本设计 | GitGit与CAS边界、S3逻辑版本分离 | 跨服务ACID假设 | BD+ADR图 |
| 详细设计 | 原子CAS/Receipt/GC/Raft恢复 | P0没有状态机/异常路径 | DD+contracts |
| 制造 | Rust crate隔离、构建/依赖审查 | 无可复现CI、密钥入库 | Build+SBOM |
| UT | 状态不变量覆盖，模型/性质测试 | 哈希/Ref CAS不正确 | Cargo test evidence |
| IT | 实机事务补偿、故障注入 | ACK Commit缺块/新Ref误回收 | fault traces |
| ST | 企业级S3/HA/WORM/性能/安全 | 迁移丢ACK/非法删/越权 | 独立ST报告 |
| 受入 | 真实制作工作流，退化体验 | 媒体历史无法恢复 | UAT签收 |
| 上线移行 | Dry-run/cutover/回退验证 | 源和目标不同步 | migration manifest |
| 运维保守 | 监控报警/DR恢复/升级兼容 | 无明确责任/恢复手顺 | Runbook及演练 |

## 2. 设计审查未关闭项
继承`../../08_quality/QA-MINIOS3-GG-001_对抗式自审.md`的P0/P1风险；技术选型不代表企业级已验证；标准S3支持集合与真实后端能力须随版本发布。

## 3. 关闭原则
“文档存在”只代表设计覆盖，不等于运行通过。每个测试实例保留CaseID、RequirementID、DesignID、Result、Build、时间戳、Test Environment、日志/hash/screenshot及缺陷ID。P0必须有真实修复和回归证据，不能直接靠风险接受关闭。
