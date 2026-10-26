# BD-MINIOS3-008 — 控制台、CLI、运营作业、告警与操作基本设计

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 对应 FR-OPS-001~015、FR-MEDIA-001~010、FR-CORE-003/004/006/008、NFR-OPS-001/002/003/006。

## 1. 设计原则

网页前端只是Rust Admin API的客户端，界面不能绕过权限或从浏览器直连data node/meta-node。导航清晰、低认知负荷、操作安全优先；默认中文，资源包支持ja-JP/en-US，术语使用日本IPA常见表达时保留中文说明。系统状态区分「实际运行」「能力已验证」「未验证」「计划功能」，不能用绿色“已支持”掩盖未测试特性。

## 2. 画面清单（画面一覧）

| 画面ID | 菜单 | 主要UI模块 | 动作/权限 | 异常与空状态 |
|---|---|---|---|---|
| SCR-001 | 全局概览 | 租户/存储池/容量/延迟/错误/迁移进度/SLO | 可读，按角色聚合 | 无指标显示数据缺失而非0 |
| SCR-002 | 租户及成员 | tenant tree、project、role、policy与quota | 管理员创建/邀请/授权 | 角色变更需影响预览 |
| SCR-003 | Git仓库管理 | 仓库/分支/commit、LFS兼容OID数、索引滞后 | RepoMaintainer维护 | Git源失联不显示虚假最新 |
| SCR-004 | Bucket列表 | Name、mode、region、version、WORM、quota | bucket管理/创建 | Backend不兼容时禁用WORM创建 |
| SCR-005 | Bucket对象浏览 | prefix/key、大小、versionId、tag、hash、retention | 授权下载/查看历史版本 | DELETE marker独立展示 |
| SCR-006 | 媒体资产详情 | Git Commit ↔ 路径 ↔ LFS兼容OID ↔ logical/physical version | 浏览、预览、授权下载、对比入口 | 原件缺失标红保留证据 |
| SCR-007 | 后端与能力 | AWS S3/MinIO/native、证书、能力比对表 | 注册、探测、测试 | capability unverified显著提示 |
| SCR-008 | 存储策略 | bucket route、allowed region、EC保护、cost | 变更申请+预览+审批 | 拒绝降级WORM/异地数据违规 |
| SCR-009 | 迁移与复制 | DryRun diff、进度、校验、approval、cutover、rollback | 提交审批、开始、暂停、续迁 | 不允许直接强制成功 |
| SCR-010 | 集群健康 | meta quorum/term, node failure domains, EC degraded, disk usage | 只读+授权修复任务 | 明确“可读不可写”状态 |
| SCR-011 | 作业与告警 | Jobs、retry、DLQ、主机事件、lag、errors | 高权限暂停/恢复 | 重试影响说明 |
| SCR-012 | 安全与KMS | IAM、STS、SSO、KMS key reference、策略审计 | security admin | 不显示secret明文 |
| SCR-013 | 合规与审计 | Retention/LegalHold、不可变审计证据、出口记录 | Auditor为主 | 外部sink落后必须警示 |
| SCR-014 | 系统设置 | 配置revision、变更历史、release、compat matrix | 平台管理员+审批 | 回滚提示实际数据影响 |
| SCR-015 | 全局检索 | media OID、git commit/path、bucket/key、tags | 按权限检索 | 显示index watermark/数据可能滞后 |

## 3. 核心操作流

**创建Bucket**：输入命名/Region→选模式/后端→自动能力检查→设置Versioning/Retention及配额→DryRun安全差异→二次确认→Rust Admin API事务创建→返回Bucket与能力证明。未经锁定能力验证不能提供“启用Compliance”成功提示。

**迁移**：SCR-009由差异表开始，展示不可迁移版本/成本/保留差异，独立审批后进行初始复制+追平+写栅栏+切换。UI状态机与Raft job状态保持一致；前端超时不意味着后端事务失败。

**媒体追踪**：由Git Repo+Commit→LFS指针路径→OID→授权链→对象logicalVersion→物理放置→校验/审计证据；允许反向从Key/OID找被哪个Commit引用（受权限限制）。对大视频提供服务器Range可播代理流但不复制原始文件到浏览器内存。

## 4. CLI命令体系（草案，最终以help规范锁定）

```text
minios3ctl system status --output json
minios3ctl tenant list
minios3ctl bucket create <name> --tenant <tenant> --backend <backend>
minios3ctl bucket versions <name> --prefix <prefix>
minios3ctl backend probe <id>
minios3ctl migrate plan --source <id> --target <id> --bucket <bucket>
minios3ctl migrate apply <jobid> --approval <approval-id>
minios3ctl migrate status <jobid>
minios3ctl storage scrub --pool <pool> --dry-run
minios3ctl git verify-repo <repo-id> --ref <ref> --read-only
minios3ctl audit export --since <utc> --format jsonl
minios3ctl support diag --redact
```

CLI写操作不得跳过Admin RBAC/四眼审批；`--dry-run`默认作用于删/迁移/配置危险动作，所有`--force`须说明只改变流程确认、不解除WORM规则。

## 5. 告警・运营KPI

| AlertID | 信号/判定依据 | 严重度示例 | Runbook动作 |
|---|---|---|---|
| AL-01 | 元数据quorum不可用 | Sev1 | 检查成员/fencing与网络，避免误启动新leader |
| AL-02 | 已ACK对象摘要损坏/丢片超限 | Sev1 | 冻结受影响key，优先恢复校验副本 |
| AL-03 | ObjectLock绕过尝试/可信时间回退 | Sev1/2 | 保留审计，停止删除/迁移高风险操作 |
| AL-04 | 复制延迟逼近RPO | Sev2 | 限流调优、后端故障排查，不能谎称同步 |
| AL-05 | 磁盘水位超阈值、EC degraded | Sev2 | 禁止危险分布写，增加节点并Scrub |
| AL-06 | Git orphan/悬挂OID、索引滞后 | Sev2 | re-run reachability和reconciliation，禁GC |
| AL-07 | 外部审计落后/导出失败 | Sev2 | 排查独立sink和outbox容量 |
| AL-08 | 迁移SHA不匹配 | Sev1/2 | 暂停对应版本，生成差异证据 |
| AL-09 | 5分钟Sev1告警未送达 | Sev1 | Alert transport冗余验证 |

监控指标严禁用对象Key/提交SHA作为Prometheus label；应在日志/trace中脱敏检索。采样日志不能成为唯一审计证据来源。

## 6. 运营手册索引

`RUN-001` first install/upgrade、`RUN-002` quota full、`RUN-003` meta quorum failure、`RUN-004` EC degraded/rebuild、`RUN-005` object integrity issue、`RUN-006` KMS outage、`RUN-007` migration pause/rollback、`RUN-008` Git LFS兼容 orphan audit、`RUN-009` WORM incident、`RUN-010` DR restore、`RUN-011` audit sink failure、`RUN-012` credential compromise。每份Runbook在详细设计包含触发、授权、观察命令、预期结果、禁做操作、恢复条件、证据与升级联系人占位。

## 7. 数据可视化与用户安全

- 呈现S3对象版本树时准确区别Live / Historical / DeleteMarker / LegalHold / Locked，历史日期统一有时区。
- media差异按钮根据文件可处理能力启用；E2前显示原图/基本信息而非伪装深度3D Diff。
- 页面清晰标示 logical_version_id和physical_version_id，普通用户默认只显示逻辑版本；provider mapping仅系统管理员可见。
- 控制台支持键盘、读屏可访问性，危险操作遵循检查清单、作业追踪号和审批码；避免多层弹窗掩盖风险。

## 8. 画面验收

按SCR-001~015逐一检查：RBAC、空状态、分页、错误、确认操作、审计记录、国际化、响应式窗口、10万条资产检索分页、弱网络恢复。所有配置变更必须通过可追踪API且写入配置revision。仅查看页面截图不等于功能验收。
