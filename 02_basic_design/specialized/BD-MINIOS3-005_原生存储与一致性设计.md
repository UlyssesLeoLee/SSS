# BD-MINIOS3-005 — Native存储节点、纠删码、元数据共识与一致性

> **v2.0生效设计边界：** GitGit原生Commit/Ref/AssetRevision为第一方权威；以下S3/Native/Raft/安全技术设计继续有效；凡提及外部Hook、Git LFS兼容或OID均仅作可选互操作。原生协议以 `BD-MINIOS3-012`、`DD-MINIOS3-014～019` 为准。


> 风险级别：P0。对应 FR-NATIVE-001~009、FR-S3-002/004/005/015/019、NFR-AVA-001/002/004、NFR-PER-001/002/003。

## 1. 原生存储总体设计

Rust `meta-node`维护线性化的逻辑对象目录、Bucket/Key/Version及Manifest指针；Rust `storage-node`持久化不可变分片与健康证据。对象DATA与元数据不存同一Raft日志：日志仅存小型manifest/ref/policy/route epoch/回收状态。存储节点通过mTLS验证服务身份，拒绝过期writer epoch与跨tenant伪造请求。节点本地索引记录stripe/shard摘要、encryption key reference、health、commit_epoch。

## 2. 默认纠删方案候选与硬件能力

| 设置 | B1初始候选 | 验证/风险 |
|---|---|---|
| 配置 | Reed-Solomon `k=4,m=2` | 逻辑冗余1.5x；具体编码实现待选型 |
| 故障域 | 3 AZ，每AZ 2个存储节点，共6节点 | 一个完整AZ失效会失去2片，正好在读取容忍边界；没有额外冗余 |
| 放置 | 每stripe 6片互异disk/node；每AZ恰好2片 | 故障域标签错误/磁盘共享需现场审计 |
| 分块 | 64MiB逻辑chunk，流式stripe+shards | 以benchmark校正；不要全对象缓存 |
| 写ACK | 6个分片durable receipt+sha验证+Raft目录commit | 即使4片可解码也不允许随意ACK |
| 读取 | 正常read→校验→可在≥4片时read-repair | 解码成功≠未损坏：全内容摘要校验不可省 |
| 故障后新写入 | 默认禁止在无法满足完整6片安全放置时发布新对象 | 可读不等于仍可写；保留E1 fault-SLO评估门禁 |

**重要**：若希望“掉一个AZ以后写入仍高可用”，6节点不是充分部署。需要冗余节点、可用容量/故障域、可在剩余可用节点上构造可持久的新条带布局，并定义降级写时的容错与SLO。任何“损失2片仍ACK新4片”的行为都需另立设计、扩大数据丢失风险证明，不允许默认打开。B1不可以在实际未达到条件前被写成企业保证。

## 3. PUT持久化协议与故障点

| 阶段 | 动作 | 持久化边界 | 故障后状态 |
|---|---|---|---|
| P0 预校验 | AuthZ、quota reservation、Bucket策略、routeEpoch | Raft准备票据与TTL | 未消耗可见版本，票据自然超时 |
| P1 输入 | 流式接收、校验长度/sha、编码+AEAD | 临时数据片可落盘不可见 | 断流产生待回收orphan |
| P2 数据保护 | 6个目标节点确认fsync/WAL durable并回传receipt | 数据在声明故障容忍包络下持久 | 未满足6片，禁止转P3 |
| P3 Commit | Raft CAS `ObjectHead`+新`ObjectVersion`+placement manifest+outbox+quota结算 | 目录commit index完成且read barrier可用 | 客户端未见ACK仍可按operation_id发现成功 |
| P4 ACK | S3 2xx/XML响应+stable logical version/etag | 已存在可读目录/持久数据 | 断线重试不再重复写坏版本 |
| P5 后台 | scrub/rebuild/replicate/index/preview | 派生/异步 | 不修改已提交对象原始字节 |

**断电故障矩阵**：P0前仅拒绝；P1/P2后P3前为orphan候选；P3后P4前“提交已成功但HTTP未知”；P4后任何故障承诺完整性。使用`OperationId`和`raft_log_index`+manifest证据判定“已成功”而不是重试写新版本。不能保证由简单超时就知道Commit是否执行。

## 4. Raft共识/读取路由

- 一个全局元数据组+若干有序Key range组；每group 3或5 voters跨3故障域，日志WAL须持久化+checksum，snapshot包含lastApplied/sequence/epoch和物理版本映射。
- Leader写入必须经过quorum committed log index，Follower不能擅自ACK写；节点/网络分区由leader term/epoch栅栏拒绝旧主。
- 强一致读取使用Raft read-index/barrier或等效已证明线性化协议；UI派生索引可以最终一致，但不能承担S3 Head/List真实性证明。
- 快照包括状态机和安全引导信息；安装snapshot/日志重放后任何已ACK对象的可见性必须恢复。保存多个快照副本，不把“有Raft”替代离站备份。
- Raft成员变更走joint/reconfiguration安全程序（使用经过验证的OpenRaft membership API），不要直接修改集群配置文件/删目录。

## 5. LIST跨分片一致性限制（阻断风险）

强一致Key单读写与强一致跨多个Raft range的分页LIST是两个不同证明问题。E0单range可建立单索引快照；E1多range须维护读栅栏和快照epoch，范围拆分期间保证无漏Key/错Token。对于“客户端在多页扫描期间同时写对象”的具体可见性语义，需契约测试承诺。**若没有形式化可用的全局列举快照设计，E1不得启用多group单桶拆分**，需退回单桶单Raft有序group并单独评估1亿对象压力。这是DD前置Gate，不可通过QA文案回避。

## 6. 物理Manifest（逻辑方案）

```json
{
  "schema_version": 1,
  "logical_version_id": "ver_...",
  "length": 21474836480,
  "sha256": "<64hex>",
  "chunk_bytes": 67108864,
  "ec": {"k": 4, "m": 2, "stripe_layout": "domain-aware-v1"},
  "placement_epoch": 44,
  "encryption": {"scheme": "AES-256-GCM", "kms_key_ref": "opaque-key-ref"},
  "chunks": [{"id": "...", "digest": "<sha256>", "shards": [{"index": 0, "storage_node": "node-a", "checksum_crc32c": "...", "durable_receipt": "..."}]}],
  "raft_commit_index": 238,
  "manifest_digest": "<sha256>"
}
```

数据节点Receipt带storage_node identity、node_generation、content_digest、fsync证明状态、placement_epoch和签名；不能信任来自客户端的receipt。

## 7. 修复、再平衡、容量管理

Scrub从目录权威索引取expected manifests，按任务优先级扫描高风险碎片，坏块立即隔离后尝试用足够健康片重建；修复片写入新generation，原片保留到元数据原子切换完成。修复的`repair_id/source_manifest/target_checksum/time/tool_version`需可审计。扩容按新旧容量权重+故障域约束逐步迁移，不做覆盖式原地改写；重平衡可限速/暂停/回滚。高水位（建议示例80%）预警、硬水位（建议示例90%）保护，应按有效空间、快照/WORM占用和EC开销计算，正式阈值需压测确认。

## 8. 数据完整性与WORM的关系

- WORM版本的“物理位置变更（健康重建）”允许仅在逻辑字节与摘要完全一致、可验证的情况下进行；不能伪造为删旧写新内容。
- 所有segments写入后持续校验、跨region复制附源摘要与目标摘要；KMS rotate时Envelope rewrap不改变对象字节，必要时重新加密分片须独立证据与授权，不能跳过保留原则。
- 元数据记录本身也受故障域保护，并通过可信时间与不可变外部审计保护留存事件。

## 9. Native错误与降级矩阵

| 故障 | 读取 | 写入 | 说明 |
|---|---|---|---|
| 单分片节点损坏 | 能重建即继续 | 仅在可创建合规布局时允许 | 需记录修复风险 |
| 一个AZ两节点失效（B1 6节点4+2） | 已提交对象≥4片可恢复读 | 默认阻断新写 | 此状态不满足“AZ故障后可写” |
| Metadata group失多数派 | 不能承诺线性化新读 | 禁止 | 可提供独立灾备只读模式需另立协议 |
| 部分分片发生bit rot | 验证+重建或报5xx | 无完整保护则拒绝 | 不可返回损坏字节2xx |
| 某Disk满 | 重新放置且满足规则才写 | 不满足则拒绝 | 不因局部满导致全局数据损坏 |
| 全局路由epoch过期 | 重试新leader定位 | 旧epoch写拒绝 | 避免双主冲突 |

## 10. 发布证明/尚待设计

实现前要求：编码器独立验证（含坏盘、位翻转）、Raft状态机模型检查、强一致LIST性能/正确性测试、placement/fault-domain机器可验证证据、1亿索引和1PiB地址空间性能模型、60min RTO演练。任何无故障注入的演示只能称功能样例。
