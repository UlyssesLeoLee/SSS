# BD-MINIOS3-009 — UT/IT/ST/UAT验收、质量门禁与可追溯性

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 设计输入为SRS v2.0 104 FR + 32 NFR + 12 UC。本文件是**测试基本设计**，不是已实施UT/IT/ST结果。逐要件追踪位于Excel。

## 1. 质量模型与阶段

| 层级 | 测试目标 | 证据/门禁 |
|---|---|---|
| 静态Review | SRS↔BD所有ID有设计责任与反向引用；风险未被静默删除 | Excel trace+review结论 |
| UT | Rust核心Domain、policy、版本状态机、checksum、事务边界 | 单元/属性/模糊用例，覆盖率≥90%目标 |
| IT | Raft/meta与data receipts、storage adapter、KMS、Git hook合同、job/outbox | 集成环境操作日志/trace |
| ST | S3 SDK回归、git-lfs CLI、千万Key列举、真实S3后端、Multi-node Native、故障注入 | 协议兼容matrix + chaos报告 |
| BENCH | B1 6节点/25GbE/1MiB GET/PUT、100MiB throughput、20GiB LFS | 实测p50/p95/p99、CPU/memory/网络/磁盘 |
| SEC | Git跨租户、sigv4、policy、WORM、KMS、sandbox、供应链 | PENTEST、SBOM、SAST/SCA |
| DRILL | 元数据恢复、源-目标切换、真实RPO/RTO、WORM恢复 | 双人签署演练证据 |
| UAT | 12个UC最终业务流程（Git、迁移、后台恢复） | 业务Owner签收 |

核心Rust模块覆盖率目标≥90%，关键安全/数据完整性分支应有100%测试设计覆盖；覆盖率由CI实际报告，不用静态文档自证。

## 2. 统一测试ID命名

- `UT-S3-<编号>`、`UT-GIT-*`、`UT-META-*`、`UT-EC-*`、`UT-SEC-*`，每个用例可追到FR、BD、Rust module/function。
- `IT-S3-*`、`IT-LFS-*`、`IT-MIG-*`、`IT-DR-*`、`ST-S3-*`、`ST-GIT-*`、`ST-NATIVE-*`、`ST-SEC-*`。
- 需求每个`FR-*`至少拥有一个正向、一个异常测试设计，关键安全/一致性/迁移需求还需恶意/故障恢复用例。
- `NFR-*`必须关联独立的量化测量程序，不将功能演示当作性能/HA合格。

## 3. 系统测试案例目录（摘要；完整案例设计需基于DD细化）

| CaseID | 场景 | 前置 | 验收/反证 | 相关要求 |
|---|---|---|---|---|
| ST-S3-001 | AWS CLI/Boto3/JS SDK CRUD | E0/Fed认证Bucket | 相同Key/ETag/HEAD/GET符合已签兼容matrix | FR-S3-001/002/003 |
| ST-S3-002 | Multipart失败并重试 | 20GiB对象随机网络中断 | part丢失可判定，Complete一次性发布，checksum一致 | FR-S3-004/008/015 |
| ST-S3-003 | 历史Version+DeleteMarker | enabled/suspended bucket | 指定版本GET正确，受保护版本未删 | FR-S3-005/012 |
| ST-GIT-001 | LFS push/pull/锁 | 4GB视频，GitGit Core事务hook | 传输、校验、Ref门禁通过；锁绕过拒绝 | FR-GIT-001/002/004/005 |
| ST-GIT-002 | Git分支删除与GC | 两个分支引用相同OID | 另一分支仍可拉取、GC不误删 | FR-GIT-010 |
| ST-META-001 | 写入ACK点断电 | 进程在P1/P2/P3/P4断开 | ACK对象一致，orphan可追踪，幂等恢复 | FR-NATIVE-005, NFR-AVA-002/004 |
| ST-NATIVE-001 | 4+2 B1 AZ故障 | 6节点，故障1 AZ/2 shards | 旧对象可重建读、新写Fail-Closed并显式告警 | FR-NATIVE-002/003/004 |
| ST-NATIVE-002 | bit rot & scrub | 手工破坏shard内容 | 识别/修复，完整SHA不变 | FR-NATIVE-008 |
| ST-MIG-001 | 千万对象迁移预演 | 含WORM/history/tag/bad provider | 差异全出、不可兼容Blocked | FR-XCHG-006/007/009 |
| ST-MIG-002 | 切换后反向同步回滚 | 目标已确认新的写入 | 不回到旧源过期状态；反向追平再切换 | FR-XCHG-008/010/011 |
| ST-SEC-001 | WORM治理 vs 合规 | Governance bypass、Compliance、Hold | 合法bypass仅匹配规范，其余拒绝 | FR-S3-012, FR-SEC-007 |
| ST-SEC-002 | 多租户OID探测 | tenant B猜tenant A OID | B无权探测/获取资产 | FR-GIT-004/008, NFR-SEC-001 |
| ST-OPS-001 | 元数据和KMS DR | 删metadata node/recover snapshot | 准确恢复映射和KMS引用，RTO/RPO实测 | FR-OPS-012, NFR-AVA-003 |

## 4. 对抗式检查用例

1. 已收到S3 PUT 200、返回VersionId，断电+切换元数据Leader之后立刻GET/HEAD/LIST，不能404或丢失。
2. 2个分片损坏后再发现1个校验不一致，系统不能将错误字节当作健康数据返回2xx。
3. `ListObjectsV2`在Key范围切分+前缀查询+有并发Put/Delete期间逐页，不可漏/重超过已承诺语义。
4. 完成Git push之前，将某条Pointer指向不存在OID，server-side Hook必须拒绝；即使禁用客户端LFS预推送钩子也一样。
5. 上传OID相同但size不同、跨租户已有OID、错误SHA内容，不能覆盖原先CAS或枚举泄漏。
6. 迁移已复制所有版本后、切换前出现新PUT且源响应200，新版本必须纳入final delta；无法确认则Block。
7. 切换后目标新PUT成功，然后触发Rollback，旧源未同步的新写入不能失踪。
8. Compliance锁住版本、设置DeleteMarker、尝试删特定Version/解除Hold与缩短保留，符合AWS的逐版本语义。
9. 停止审计sink，再发生强制解锁/删除尝试；不能因为投递失败丢掉权威outbox事件。
10. 用不可信Glb/PSD/视频攻击预览worker，不能接触KMS或宿主文件和其他租户对象。

## 5. B1 Benchmark测试设计

输入B1：6 storage nodes (16vCPU/64GiB/NVMe/25GbE)分布3故障域，API/meta节点独立；记录每个对象大小、数量、读写比、总量、对象热度、加密/版本状态。GET 1MiB p95≤150ms、PUT≤300ms、100MiB连续读聚合≥1GiB/s为SRS目标而非现已达成。环境变更必须重新建立基线；S3 Federation另报平台额外延迟/Provider延迟，不能混为原生吞吐。

## 6. 故障注入矩阵

| 故障编号 | 注入 | 预期 | 证据 |
|---|---|---|---|
| CH-001 | kill Meta leader at raft commit | quorum恢复、ACK版本一致/未ACK可核验 | raft commit index日志 |
| CH-002 | AZ失联2 nodes | 4+2下已提交读可用，写安全受限 | EC manifest/shard日志 |
| CH-003 | 提交后网关网络断开 | 幂等重试返回可解释版本 | operation ledger |
| CH-004 | ObjectLock后时钟回拨 | retention不缩短，阻断不可信操作 |可信时间事件 |
| CH-005 | Upstream 429+网络限速 | 租户隔离、重试有界、checkpoint正常 | worker lag/queue |
| CH-006 | Kubernetes滚动升级N/N-1 | metadata和响应语义兼容/可恢复 |升级前后diff |
| CH-007 | 删Git Ref前丢Webhook | reconciliation修复、不清理OID | Git signed refs snapshot |
| CH-008 | Outbox sink断开 | 事件仍可恢复重送、不伪报审计完成 | outbox offset/digest |

## 7. 缺陷分级与发布条件

- `P0 Blocker`：已ACK对象丢失、WORM误删、跨租户读取、错签名放行、双主写入、Git引用被GC破坏。
- `P1 Critical`：支持的S3 SDK接口重大不兼容、迁移覆盖确认数据、Meta quorum反复脑裂、RTO/RPO超出已签目标。
- `P2`：可恢复的非安全功能缺陷，有明确临时措施/追踪编号。
- 发布E1至少P0=0，P1=0或风险责任人书面批准（数据完整性/安全领域不可例外），覆盖率和兼容矩阵附真实报告。合规认证与第三方测评未完成时必须明示不能对外宣传。

## 8. 证据与追踪关系

Excel每个FR/NFR包含设计文档ID、负责模块、阶段、测试设计ID、验收方式、评审状态（默认Draft）、证据URI占位、失败处置。未执行测试不能填“Passed”；静态检查“覆盖所有ID”仅表示设计索引齐全。
