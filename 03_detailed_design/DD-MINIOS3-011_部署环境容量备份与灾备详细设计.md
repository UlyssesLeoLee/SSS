# DD-MINIOS3-011 — 部署环境容量备份与灾备详细设计

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：VM/K8s、网络、配置、RTO/RPO与升级


## 1. 环境与拓扑

| Environment | 元数据 | 数据/后端 | 安全/依赖 | 发布目的 |
|---|---|---|---|---|
| DEV | 1 meta local group（**无HA**） | Fed测试桶/本地mock | 测试OIDC/KMS mock | Rust UT/调试 |
| IT | 3 meta voters | AWS S3 + 自建S3，6模拟节点 | 受控TLS、Git staging | 真协议互操作 |
| ST/B1 | 3/5 meta voters分布3AZ | 6×16vCPU×64GiB×NVMe×25GbE（候选），4+2 EC | 实KMS/IdP/审计sink | 故障/性能/DR资格验证 |
| PROD E1 | 由B1实际结果最终确定 | 必要时增加备用Node支撑目标读写SLO | 独立admin平面与审计 | 正式发布（尚未达到） |

公网断开测试必须保证选择本地可达Fed/Native后端；连接AWS S3的Federation不可假设离线仍读写。K8s PodDisruptionBudget不是Raft多数派证明；部署前检查真实AZ/机架/磁盘隔离。

## 2. 网络信任区与端口

S3、LFS公开TLS入口；Admin与S3使用不同DNS/Ingress和WAF策略；服务间mTLS端口用网络策略限制特定SA；Meta集群Raft节点不能直接被Client访问；KMS出站、S3上游、审计sink严格允许清单。LB须透明保留原始Host/path/query及SigV4 SignedHeaders；代理重写导致验证失败应作为部署阻断条件。

## 3. 配置结构与升级规则

规范配置样例见`06_examples/minios3_values_design.yaml`。配置类型区分：RestartRequired/HotReloadable/ApprovalRequired/ImmutableAfterCreate。Pool EC布局、创建时Object Lock启用方式和外部桶映射不可任意热改；RouteEpoch变更必须通过Admin批准的Job，不允许修改ConfigMap直接切换。日志SecretRef必须遮蔽。

## 4. 存储容量模型与性能预算

粗估 `physical ≈ logical_active × (k+m)/k + historical_versions + retained_versions + replicas + transient_migration + snapshots + repair_headroom`，各项不能机械相加的重复部分需在Sizing表实际计算；Quota以物理/逻辑双指标呈现。20GiB上传的每请求RSS应按`chunk_size × max_inflight_chunks × shards_buffer`设上界，控制面不做O(ObjectSize)分配；至少3租户公平性及KMS延迟影响要基准测试记录。

## 5. 备份和恢复闭环

不能只备份Raft：需要四联恢复包 `metadata snapshot/WAL + object shard/native manifest或provider version映射 + GitGit Core refs/签名快照 + KMS key/审计证据`。每个恢复点带全局一致性`raft_index,manifest_digest,git_snapshot_watermark,outbox_watermark,kms_key_ref`；备份服务账号只拥有最小只读+备份目的写权限。恢复必须验证原文SHA与LFS可达引用，并重建索引而不是把空搜索误判为丢对象。

## 6. DR/Failover/Failback

恢复时先Fence旧主（或证明已失写权）→恢复Meta group snapshot→校验storage receipts/版本manifest→KMS恢复访问→Git Ref watermark对账→只读验收→选定新主routeEpoch→受控开放写入→验证RPO/RTO并留签署。跨站异步若lag>RPO目标，应标记潜在数据窗口，不得称零丢失。季度完整DR+月度恢复抽检属于NFR待执行验收。

## 7. 滚动升级与失效回退

顺序：Backup/checksum→新旧Proto协商→可回滚schema准备→按Raft voters逐个滚动并维持多数派→Gateway/Worker灰度→检查可读/可写、审计watermark→完成或回退。不可逆metadata schema仅在所有实例切至N+1后审批启用；N/N-1 mixed-version兼容至少通过IT/ST矩阵。

## 8. 发布物安全

Linux amd64/arm64与K8s受测；CI必须产出带签名的容器/二进制、Cargo.lock、SBOM、依赖许可证与CVE结果、可复制的离线镜像安装说明；Windows只承诺CLI/客户端（Rust服务Windows运行不在基线）。
