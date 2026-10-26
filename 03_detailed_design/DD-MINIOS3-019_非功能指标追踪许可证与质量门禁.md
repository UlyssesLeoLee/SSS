# DD-MINIOS3-019 — 非功能性、可靠性、OSS许可证和质量门禁

> v2.0 | 所有新增NFR-GG-001～012 | 设计状态Not Implemented

## 1. 目标

GitGit+MinIOS3 E0业务闭环和E1企业级质量均须有环境固定的SLO、RTO/RPO、并发样本、负载规模和原始测量数据，参照日本IPA《非機能要求グレード2018》维度管理。不可将概念图或mock单元用例称为性能/可用性证明。

## 2. 压测计划

以20GiB流式大媒体（非全部驻内存）、1GiB早期插入后的CDC去重率、千万AssetRevision索引、百万Chunk回收竞态、6节点4+2容错及实际S3 Provider 429/503作为负载；硬件与网络基线要记录实际配置，未约定阈值标`Needs Agreement`。

## 3. 可靠性证明

GitGit Ref CAS失效、MinIOS3 Chunk少片、断电、Raft leader交接、Outbox延迟、迁移Cutover并发写、KMS拒绝、WORM合法保留：凡已ACK的Commit/Manifest不得在声明故障域内丢失或错误授权。失去多数派或不可满足EC门禁时写入FailClosed。

## 4. 开源商用合规

Rust/前端/独立二进制/模型及字体素材分层生成SBOM，依赖真实发布版本的SPDX/NOTICE检查；禁止把单纯开源等同宽松商用许可。MIT/Apache/BSD优先，MPL/其他弱Copyleft按文件修改义务审查，GPL/AGPL/FFmpeg的构建选项必须单独审查；涉及编解码专利和云费用另计。

## 5. 门禁

UT变更代码行/分支覆盖率目标≥90%（采用cargo-llvm-cov，若达不到需有评审批准），IT关键协议互操作已通过真实二进制，ST所有MUST场景无P0/P1未解；License Gate / WORM legal / GitGit原子PoC / 多媒体format matrix 未签署不得标称企业级正式GA。
