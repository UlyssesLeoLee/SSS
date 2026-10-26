# BD-MINIOS3-010 — 部署、环境、备份与运行容量基本设计

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 对应 FR-CORE-001/002/005/007、FR-NATIVE-001/003/007、FR-OPS-001~015、NFR-ENV-001~004、NFR-AVA-001/003/006。

## 1. E0/E1环境矩阵

| 环境 | E0开发/联调 | E1生产参考 | 限制 |
|---|---|---|---|
| OS/Arch | Linux amd64/arm64 | Linux amd64/arm64经验收 | Windows只支持CLI/SDK，不作为原生服务正式目标 |
| 部署 | 单进程开发模式/VM/容器 | Kubernetes或VM多AZ | 单机不可宣称HA |
| 元数据 | 1 voter（相同Raft状态机） | 每组3或5 voters，跨3域 | 剩余多数派不足禁止写入 |
| S3 API | 1+instances | 3+ 无状态instances + LB | 主机名与签名必须保持一致 |
| LFS API | 可与S3 API共部署 | 根据流量独立拓展 | 必须支持长传输和GitGit Core Hook |
| Native节点 | E0不启用 | B1至少6，生产按故障期可写SLO增冗余 | B1 AZ损失2 shard后不可全冗余新写 |
| Task workers | 1 | ≥2（迁移/GC/预览资源隔离） | Worker任务操作幂等 |
| KMS/IdP/Audit | stub仅供测试，不能产品认证 | 企业KMS/IdP+外部WORM sink | 断外网前需有可信内网依赖 |

## 2. 网络与安全区域

```
Client Internet/Intranet
        ↓ TLS
  Load Balancer (2+)
        ↓
 API Gateway / LFS / Admin API (不同身份和RBAC)
        ↓ mTLS / private
 Metadata Raft Nodes <----> Storage Nodes (gRPC mTLS)
        ↓                      ↓
   Worker jobs              Encrypted disks
        ↓
  Provider S3 / KMS / Audit sink (egress allowlist)
```

管理平面独立入口、MFA。东西向仅允许服务身份。所有stateful数据盘具有明确PVC/VM磁盘绑定和故障域label，Storage data与Metadata WAL不共用容易互相干扰的磁盘。生产建议按容量/吞吐记录每类磁盘预算并单独为audit outbox设置水位。

## 3. 参数分类与配置安全

- **集群只读/启动参数**：节点id、join endpoint、TLS CA ref、data roots、worker runtime；更改后需Rolling plan。
- **强一致全局配置**：租户、Bucket策略、Backend路由、EC布局、版本、retention、IAM、quota；在Raft中保存revision、approval与有效期，变更发出epoch。
- **Secrets**：仅用secretRef引用KMS/IDP/Backend凭据；不可在Git提交明文secret yaml。
- **feature flags**：默认禁用未在兼容矩阵验收的S3 API；flag不能削弱WORM或AuthZ。
- **回滚**：组件二进制回滚依次检查meta schema版本与协议兼容；不可逆schema变更提前备份并计划前向修复，而不谎称任意版本可降级。

建议配置例见`../06_examples/minios3_values.example.yaml`。

## 4. 容量与性能尺寸设计

有效逻辑容量粗算：对于4+2 EC，纯数据分片放大1.5x，另加Object版本、WORM保留、metadata、碎片冗余、迁移双存、快照和空闲水位。举例1PiB逻辑唯一原始字节（非实测）至少约1.5PiB数据物理片容量，**不包含**历史版本、可用水位、故障域保护空间及备份。吞吐受EC编码CPU、NVMe、Raft写入次数、KMS延迟及网络限制，按B1验收，不从网络25GbE直接推算出承诺性能。

## 5. 发布/升级与回滚

1. CI生成 signed OCI image、SBOM、compat manifest、语义迁移说明、测试报告与锁定的Cargo版本；审批生产变更。
2. 灰度1个无状态Gateway校验S3 SDK+Git LFS兼容基础路径；后更新Workers和Meta followers，再对Raft leader迁移/升级；Storage节点逐故障域滚动，避免同时失去EC必要片数。
3. 在storage/metadata schema/format更改前创建可验证备份与恢复演练。任何升级不能让旧节点误识别新Manifest而默默写坏数据。
4. 元数据migration若不可逆，不允许通过直接降低二进制版本“回滚”；走支持的forward fix或者经过演练的restore流程。

## 6. 备份体系

- 权威metadata：加密Raft snapshot+WAL恢复点+对象/块manifest+路由epoch，校验后持久化到外部WORM或保护存储。
- 数据：按受控版本复制/冷备与定期校验；保持S3对象原始字节和Git CAS OID，目标后端不可满足retention则该备份不能称等价合规副本。
- 密钥：KMS KeyRef、密钥轮换/恢复程序（不把主密钥明文放进备份zip）、TLS可信根、IdP映射。
- Git：GitGit Core原始Repo Commit/Ref权威由其自身备份机制保证，MinIOS3只备份与仓库的映射；Restore后重新运行refs reconciliation。
- 演练：每月抽样metadata+object恢复；季度灾备切换、full integrity复核、时间和审批证据。

## 7. 运行值班与容量阈值

存储容量>80%预警、>90%阻断危险扩容/低优先级作业的示例阈值不是正式NFR数字；生产以压测/故障容忍必须预留多少空间决定。告警可配置但不得关闭P0故障场景（Meta quorum, ACK object corruption, WORM bypass, KMS不可用）。合理暴露Sev1自动报警≤5min。审计保留目标≥365天为原SRS候选值。

## 8. 脱机部署与灾难恢复前置

提供Rust发行二进制、签名OCI镜像、安装manifest、离线CA/证书申请步骤、Sbom、许可证/声明文件、紧急恢复手册；外网不可达时，现有内网IdP/KMS/审计依赖满足声明条件才可能操作。若provider S3在公网，断公网时其数据不可用属后端依赖，而不能虚称Federation完全脱机可用；E0 `FR-CORE-001`须按“原生/本地Fed后端具备依赖”的测试范围验收。

## 9. 详细设计/运维签署清单

所有IP/DNS/Port和容量来自实际部署合同，不在BD硬编码用户地址。等待决策：真实跨AZ类型、读写SLO拆分、Raft 3还是5 voters、Native纠删布局、备份目标、KMS供应商、IdP、离线许可证、日志实际保留、Docker/K8s/VM共同覆盖矩阵。每项变更同步需求、BD、DD、UT/IT/ST设计书。
