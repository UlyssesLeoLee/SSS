# OPS-MINIOS3-001 — 運用設計及标准操作手顺（Runbook）

## 1. 运维角色与职责
SRE负责节点/容量/告警，Security负责鉴权/审计/KMS，StorageAdmin负责bucket/route，RepoAdmin负责GitGit仓库策略，媒体制作人员仅管理被授权资产。执行生产破坏性动作要求二人复核与操作票。

## 2. 监控SLI（阈值待ST实测冻结）
| 维度 | 指标/日志 | 失效判定 |
|---|---|---|
| GitGit | RefCAS成功/冲突、Outbox backlog、不可还原Commit | 任何已ACK Commit读失败按P0处理 |
| CAS | manifest hash mismatch、missing chunk、receipt lag | 一次读到损坏内容需隔离并升级 |
| Storage | 每节点磁盘容量/EC degraded sets/self-heal progress | 自动修复无法收敛或余量不足 |
| S3 | S3 API错误分类、P99 latency、Multipart orphan | 服务SLO和用户体验退化 |
| Security | policy denies、Token misuse、Key rotation、WORM拒绝 | 未授权读成功或保护版本误删即P0 |
| Migration | route_epoch、copy/verify backlog、tail watermark | Cutover后尾差增长阻断删除旧源 |
| Worker | sandbox denials、decoder crash、job queue | 媒体预览故障不能阻断原件 |

## 3. 标准作业
日常：检查Raft quorum、故障域分布、EC修复、Outbox对账、GC deferred列表、后端限流及容量趋势。每周：验证随机历史Commit checkout和hash、备份完整性与副本地理策略。每月：在隔离环境执行metadata restore、KMS轮换演练、版本发布漏洞检查。所有频率为建议值，应与实际SLO协商确认。

## 4. 故障等级
P0：已ACK数据不可读/失真、非法越权、WORM失效、不可证明的迁移数据丢失；立即阻断删除与切换，保全审计。P1：读写高错误率、Raft不稳、修复落后；限流、隔离故障域，启动应急预案。P2：预览失败、索引延迟；降级但不影响原件。

## 5. 数据恢复
仅在隔离环境中确认备份元数据、恢复映射、取回任意历史媒体后，才可宣布灾备演练通过；并应将恢复后的Ref/Manifest Hash与备份时证据比对。
