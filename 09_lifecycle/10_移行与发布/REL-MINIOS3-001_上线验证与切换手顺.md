# REL-MINIOS3-001 — 发布准备、上线与回退手顺

> 各步骤为待实现的运行程序设计，不应在无实机演练时直接应用到生产。

## 上线前核对
- Go/No-Go：E0/E1/E2适用测试报告，P0=0；证据由独立QA确认。
- 软件：Rust构建hash、镜像签名、SBOM/NOTICE、S3 SDK回归矩阵和迁移资格。
- 数据：权威元数据快照及restore proof、Manifest样本sha256、WORM/LegalHold统计、routeEpoch记录。
- 安全：TLS/Keycloak/KMS与凭据轮换流程、SecReview、密钥访问拒绝测试。
- 容量：EC布局、磁盘预留、自愈水位、备份和监控告警阈值。

## 上线窗口操作顺序
1. 变更审批；创建不可变部署记录和监控基线。
2. 验证所有成员节点版本一致/兼容；启动读健康巡检。
3. 流量按租户或canary桶渐进切换，核验PUT/GET/Range/GitGit commit/checkout。
4. 完整比对旧/新路由VersionId映射及媒体原始哈希；检查Ref CAS冲突并发场景。
5. 锁定变更窗口结束点、通知用户，并签收运维资料。

## 回退策略
升级若涉及数据库Schema、密钥或WORM不可逆操作，禁止未经演练直接回退二进制。若迁移后新写入已ACK，必须先进行反向增量同步并重新校验；不同步则停留新版本并执行故障处置，不得“快速改回旧URL”。
