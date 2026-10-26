# REQ-MINIOS3-002 — 要求分析、优先级与可验收性设计

> 权威要件列表：`../../01_requirements/SRS-MINIOS3-GG-001_要件定义书_v2.0.md`。本文件补充判定方法，不复制/覆盖ID。

## 需求层级
业务要求BR → 用例UC → 功能FR／非功能NFR → 基本设计BD → 详细设计DD → UT/IT/ST/UAT → 实测证据。任何变更通过`00_governance/change_control_v2.0.json`追踪；原编号不复用。

## 特性验收的断言
- 版本正确性：从任意受保留Commit检出、重新SHA-256校验、重启/迁移后结果一致。
- 内容正确性：Manifest的总大小、chunk序、偏移、原件hash与实际字节完全一致；拒绝重叠或缺口。
- 事务正确性：缺少DurableReceipt/过期PolicyEpoch/expected-ref不符应失败且旧Ref保持可见。
- 故障安全：GC对不确定可达性必须保守拒删；安全审计记录可追踪操作、身份及策略决策。
- 兼容性：逐API/SDK的S3 profile独立发布，不能把“支持PUT/GET”理解为全S3兼容。
- 部署正确性：孤立网络中的GitGit+MinIOS3原生全链路可用，LFS/外部Git非必要。

## 度量冻结规则
性能/容量/可用性目标存在SRS内已有声明、环境变量依赖或待审项时，必须在ST环境设计书记录：对象大小分布、并发度、网络RTT、S3 backend类型、节点数、故障域、样本、基线软件版本、测量时间、可信区间及缺失风险；未经签字冻结不得宣称达标。

## 缺口与变更触发
高危未决：多分片Bucket LIST强一致性、Raft日志持久化与Snapshot、WORM法规边界、实时Commit原子门禁、20GiB媒体极端路径、跨后端迁移后的回滚窗口。任何改动上述不变量的提案，必须更新SRS+BD+DD+测试三层并要求对抗复核。
