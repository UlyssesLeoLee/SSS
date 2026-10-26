# BCP-MINIOS3-001 — BCP / DR / 备份与恢复设计

## 1. 备份对象
GitGit Repo/Ref/Commit/Tree、权限策略、租户路由、MinIOS3 Manifest/VersionCatalog/Retention、KMS密钥引用、索引重建配方和审计日志分别管理。备份不复制生产密钥明文；密钥恢复由独立KMS流程控制。

## 2. 故障模型与恢复次序
机房损毁、元数据丢失、对象损坏、恶意删除、密钥不可用和运维误路由分开建模。恢复序列通常为：隔离故障→冻结GC/路由变更→恢复元数据快照与日志→验证KMS授权→恢复Chunk/Manifest可达性→恢复GitGit Ref与Outbox并对账→采样/全量SHA256校验→授权恢复业务。

## 3. RPO/RTO
数值由产品和运维合同批准，计划不擅自设成0。已确认Commit在所宣称故障域内的恢复正确性与跨区域灾备RPO是不同的保证；需要记录同步复制或异步备份机制。

## 4. 演练场景
- 单节点/多磁盘损坏及自愈、Raft leader crash；
- 整个故障域失效后业务读写能力评估；
- 误删GitGit Ref和被保护媒体恢复；
- 完整恢复某个历史Commit的媒体内容与SHA256；
- S3跨后端cutover后恢复旧集群并防止新ACK版本丢失。

## 5. 演练证据
记录备份点、恢复起止、实际RPO/RTO、是否有未可达对象、Ref/Manifest比对结果、S3版本逻辑映射、审批签字。当前设计未进行实机演练。
