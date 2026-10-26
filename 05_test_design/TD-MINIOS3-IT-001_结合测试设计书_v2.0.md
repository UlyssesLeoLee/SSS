# TD-MINIOS3-IT-001 — 結合試験設計書

> v2.0 / Not Run | 194条IT用例

## 1. 合作方及真实服务界面

**必需联调：** GitGit Native Rust Core ↔ MinIOS3 Chunk/Manifest Gateway ↔ Raft Meta ↔ Storage Native/Federation；GitGit Commit Coordinator ↔ Durable Receipt Validator ↔ Ref CAS Outbox；MediaAdapter Worker ↔ GitGit Review/AssetIndex；Web UI ↔ AuthZ ↔ GitGit APIs。

**可选兼容：** 原生git-lfs CLI ↔ 独立GitLFS适配器；标准Git pack/wire导入器。可选兼容缺失不得冒称主链路已经验证。

## 2. 关键故障断点与预期

| 断点 | IT预期 |
|---|---|
| 上传块缺一块或fsync失败 | ManifestSeal拒绝，GitGit Ref不可发布 |
| Manifest确认完成而Commit失败 | 块暂时受保护，按TTL/GC规则补偿；旧Ref不变 |
| Ref CAS提交成功后HTTP ACK丢失 | 幂等查同一Commit，Outbox最终补全 |
| 并发抢占同一RefRevision | 仅一端成功，冲突端明确反馈 |
| Chunk在GC与新引用之间竞争 | 双阶段安全栅栏不误删 |
| Adapter崩溃/无法解析媒体 | 原件/历史仍可恢复，预览标Failed |
| 复制/迁移S3 PhysicalVersion变化 | GitGit Revision、Manifest SHA及Ref关系不变 |
| 租户A试图猜取租户B Chunk | 禁止存在性枚举和读取 |

## 3. 真实Provider

对S3必须在兼容矩阵标注AWS CLI/boto3/aws-sdk-rust与自建Provider的版本+功能格；GitGit协议用自身Rust SDK/CLI断点联调。不将模拟仓库事件替代真实Ref Store CAS。

## 4. 环境与证据

隔离Namespace、2租户、2仓库、至少2用户与媒体goldens；每条保存OpID/RefRevision/Manifest/Chunk SHA、Raft日志epoch、SDK版本、HTTP trace(脱敏)、缺陷票据。见`IT_完整案例明细_v2.0.md`。
