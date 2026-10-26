# DD-MINIOS3-010 — 任务调度运维控制台与管理CLI详细设计

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：Worker、运营UI、日志告警与API管理


## 1. 统一Job调度和Lease

Job以Raft保存`job_id,kind,tenant,priority,parameters_digest,revision,phase,lease_epoch,lease_until,cursor,progress,error_code,approval,created_by,updated_at`。Worker领取时CAS绑定`lease_epoch`，旧worker即使未退出也不能覆盖新worker checkpoint。每个batch按`op_id`记录结果；超时重试只能在确认前步操作结果后继续。DLQ中保留不可恢复项和审计原因，重试不能自动越过安全Blocked。

## 2. Job类型与队列

| Job | 触发 | 幂等依据 | 暂停条件 | 拒绝自动继续条件 |
|---|---|---|---|---|
| Migration | Admin+审批 | job+version+destination | 429/容量/预算 | Retention差异、源版本不可读 |
| Replication | Committed outbox | source event id+target version | 后端不可用 | region政策不符 |
| Scrub/Repair | 定时/告警 | manifest generation+shard id | 前台读写压力 | 小于k健康片/不合法layout |
| LFS Reconcile | Git snapshot | repo+snapshot digest | Git连接断开 | 签名/水位无效 |
| Git GC | 双阶段计划 | candidate+snapshot epoch | 无新snapshot | 活跃引用/保留/hold |
| Preview | CAS Verified | source_oid+transform_version | 沙盒池满 | 恶意文件隔离 |
| Audit Export | Raft outbox | event_id | sink不可达 | evidence integrity mismatch |
| Backup/DR | Ops窗口 | snapshot checkpoint | 带宽限额 | KMS与对象manifest不一致 |

## 3. Admin REST与CLI契约

管理API以输入BasicDesign `04_contracts/admin_api_openapi.yaml`为基线，详细补充稳定字段和`Idempotency-Key`；管理写操作必须校验`If-Match: config_revision`或对应expected_rev；禁止无条件覆盖。CLI通过同一API鉴权并写审计，不允许直写Raft KV。危险操作`request -> preview -> different-person approve -> execute -> evidence`，失败时给可操作的runbook链接。CLI命令族：`cluster, bucket, versions, backend, migration, git, audit, config, backup, diagnose`，参数示意非可运行源码。

## 4. 页面信息架构（与基本设计SCR清单对应）

| 页面 | 信息与交互 | 权限限制 | 危险动作防护 |
|---|---|---|---|
| Bucket/对象浏览器 | 历史Version/Retention/标签/原文摘要/Key精确字节 | tenancy+scoped policy | 删除特定version二次确认 |
| Git版本关联 | Ref→Commit→Path→OID→Version→Placement | repo scope | Stale watermark醒目 |
| Backend能力页 | Feature可用性/验证版本/证据 | Ops/Auditor | Unverified禁止被标绿色Supported |
| Migration工作台 | 阶段、差异、成本、水位、rollback边界 | Owner/Approver独立 | 双人审批和Fence状态 |
| Cluster健康 | Raft group、quorum、EC、zone容错、容量 | 运维只读 | 可读/可写两状态分开 |
| 审计追踪 | op_id、GitRef、OID、Version、Sink回执 | Auditor | Secret脱敏+导出水印 |
| 媒体 | 原文下载、独立派生预览状态 | asset授权 | 沙盒失败不隐瞒 |

## 5. 多语言和可用性

UI资源key稳定，支持zh-CN/ja-JP/en-US；术语统一：要件定義/基本設計/詳細設計、保持“对象版本”和“Git版本”的不同。页面按读者角色默认展示任务重点，错误显示稳定ErrorCode+相应建议，不展示原始stack或隐私数据。危险操作具键盘可达提示、上下文明确和预览影响范围。

## 6. 指标/警报基线

`requests_total`,`request_duration_seconds_bucket`,`raft_commit_latency`,`raft_quorum_state`,`ec_healthy_shards`,`audit_outbox_lag`,`git_event_lag`,`migration_checkpoint_lag`,`replication_rpo_seconds`,`backend_429_total`,`kms_errors`,`preview_failure`,`pending_gc`,每项带低基数维度；禁止Object Key/OID作为Prometheus label避免标签基数泄漏。Sev1 5min为NFR拟定目标，告警测试记录触发、投递与值班响应者。

## 7. Runbook目录

RB-01 meta quorum失败、RB-02 zone失效只读、RB-03 shard checksum错误、RB-04迁移Cutover阻断、RB-05 cutover后逆向回滚、RB-06 Git events滞后/GC冻结、RB-07 KMS中断、RB-08审计sink中断、RB-09容量满、RB-10 rolling upgrade撤回、RB-11恶意媒体沙盒异常、RB-12备份恢复与外部依赖验证。每个Runbook记录Precheck/执行人/审批人/命令与输入/回退与证据/通信方式。
