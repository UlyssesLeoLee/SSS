# MIG-MINIOS3-001 — 移行設計・データ移行計画

## 1. 移行类型
(1) GitGit新建Repo数据导入；(2) 既有媒体及外部Git/Git LFS可选迁入；(3) MinIOS3内部Native与S3 Federation间互换；(4) S3后端迁移。不同场景采用不同Id映射，禁止物理VersionId替代逻辑ID。

## 2. 执行窗口与先决条件
迁移前冻结需求、记录源/目标S3兼容能力、LegalHold保留规则、所有VersionId和DeleteMarker的处理策略，生成对象/版本清单及全量摘要。取GitGit Metadata+Audit快照并验证从备份可恢复；迁移过程中对每个bucket/tenant记录route_epoch。

## 3. Runbook
| 顺序 | 动作 | 判定与失败处置 |
|---|---|---|
| M01 | 目标后端功能比对、ACL/KMS/Object Lock可行性 | 不支持强制策略则阻断 |
| M02 | Dry-run枚举、列举所有可达版本和旧映射 | 写只读差异报告 |
| M03 | 初始拷贝/去重验证 | 每个Manifest SHA与逻辑版本一致 |
| M04 | 开始变更捕获并增量追平 | 使用watermark，处理重复/乱序 |
| M05 | 切换前栅栏或受控双写 | old-route写者被拒或重试到新epoch |
| M06 | 追平最后尾差、校验保留语义 | 未追平不可Cutover |
| M07 | 更新唯一稳定Endpoint映射 | 存储GitGit↔S3版本链完整 |
| M08 | 监视读写、历史恢复、审计和错误率 | 记录回滚阈值 |
| M09 | 如需回滚先反向追平新后端的已ACK写入 | 若数据完整性不可证则阻断回滚 |
| M10 | 保留源后端和对账证据至宽限期 | 过期后的删除也须遵守Retention/WORM |

## 4. 源码及元数据迁移
标准Git导入功能是E2适配器；若GitGit无法映射外部Commit结构，须输出明确不可迁移清单，不得伪造历史关联。原件与派生预览分开处理；派生可再生，但其生成参数仍应保留以重现。

## 5. 证明记录
必须保存manifest/version映射、对象总数/总字节数、抽查及全量hash、失败与重试、S3物理版本差异、Cutover审批、已ACK事务尾差处理和回滚日志。
