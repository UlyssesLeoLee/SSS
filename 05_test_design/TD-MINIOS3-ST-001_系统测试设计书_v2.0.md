# TD-MINIOS3-ST-001 — システム試験設計書

> v2.0 / Not Run | 249条ST用例

## 1. 业务端到端集合

以GitGit原生桌面/CLI/Web工作流为主：10GiB视频变更增量提交与恢复、分支和人工冲突处理、音频区间/图像热区/3D资产比较、依赖影响分析、审批发布、离线编辑及历史追溯、仓库导出。

## 2. 企业级可靠性/安全

结合旧版S3 Versioning、WORM、Raft 4+2、跨AZ、RTO/RPO场景，增加 **已ACK Commit绝对不可引用缺块**、跨存储迁移后全部GitGit Commit恢复、GC与离线工作区/Tag/Review竞态、版权阻断、沙盒恶意媒体、ACL撤权等系统级Gate。

## 3. 压力与NFR

20GiB真实流式读写、1GiB插入前段的CDC效率、千万AssetRevision图、百万素材Batch import、3+节点元数据和6节点EC均须记录机器/网络规格与原始样本。凡阈值尚未由业务签署，Case维持Not Run / Needs Agreement，不能标Pass。

## 4. WORM和OSS许可

受保留对象不可用管理面绕过删除；S3物理VersionId改变不破坏GitGit逻辑版本；独立Worker的FFmpeg二进制、Rust crates及JS依赖均需生成SBOM与许可证审查记录，涉及编解码专利另审。

## 5. 验收

所有适用P0/P1发现关闭、完整SHA/Commit图可复算、严重安全越权0；详见`ST_完整案例明细_v2.0.md`和Excel执行栏，**无实际测试运行**。
