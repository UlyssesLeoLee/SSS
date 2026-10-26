# DD-MINIOS3-005 — Native纠删码数据面详细设计

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：E1 Native、EC、placement、scrub与容量控制


## 1. 数据组织与编码参数

E1 B1候选值：`EC k=4,m=2`、64MiB原始逻辑chunk、六个storage-node按3个AZ各2节点。**候选不是经过证明的参数**；须编码性能与故障模型签署后冻结。编码以小stripe分块流式进行，不能装入整64MiB或20GiB对象进每请求内存；分片采用不可变generation。

Manifest包含 `object_version, original_length, original_sha256, chunk_size, codec_id, k, m, fault_domain_policy, placement_epoch, manifest_digest, kms_ref, per_chunk{index,logical_size,chunk_sha256,shards[]}`，Shard包含`node_id,disk_id,az,seq,generation,stored_crc32c,AEAD_nonce_ref,durable_receipt`。

## 2. 写入状态与Durable Receipt

1. `NativePrepare`在meta取得routeEpoch、quotaReservation、预定Shard placement；所有节点使用mTLS与节点世代校验。
2. 每一个逻辑chunk计算原文SHA-256；加密方案明确顺序为**原文→压缩（若启用须注明算法）→AEAD加密→EC编码**，还原逆序；同一对象的整体SHA-256校验放在完整请求或读完整对象时，Range至少校验可验证的chunk/AEAD。
3. 写数据片至磁盘临时segment；fsync data+index+journal后产生由storage-node签名/认证的Receipt，字段含ticket、node_generation、shard_digest、durable_sequence、placement_epoch。
4. **所有六片receipt齐备**且故障域符合→meta `CommitPut`→返回2xx。只收到4片绝不ACK，以防下一处故障导致数据低于安全目标。
5. Commit后任何数据节点重启按manifest逐条核对，不得把未发布orphan当已提交对象；孤儿GC必须经过元数据安全确认。

## 3. Placement算法（确定性约束）

输入：`k,m, zone_count, available_nodes, healthy_disks, capacity_reserve, placement_epoch`。算法先过滤disk阈值与过期node/zone，按AZ分配slot，再按节点隔离与权重产生每片位置；必须保证本策略的每片位于独立Node/Disk，恰好2片/AZ(当前B1)。若不足6个健康节点/容量，返回`InsufficientFailureDomains`，不会退回随机同盘放置。为避免热点，生成的计划带分布权重和验证哈希，但不依赖未随机化安全种子保护对象。

## 4. 故障与读修复

| 情况 | 判定 | 对外读取 | 新写入 | Scrub/Repair |
|---|---|---|---|---|
| 正常6/6 | 所有片校验 | GET/Range | 按安全策略允许 | 后台抽检 |
| 单节点故障5/6 | 至少4片可用 | Reed-Solomon重建读 | 只有新placement满足6片才允许 | 重建到新disk |
| 单AZ故障4/6 | 现有片正好k片 | 可尝试读取并严校验 | 默认**拒绝**新写；非连续写HA | 新AZ/备用容量恢复后重建 |
| 片位翻转 | CRC或AEAD不符 | 仅有≥k正确片才可重建 | 受健康水位影响 | 隔离错误片 |
| 同时3片不可用 | 少于k | 返回5xx+数据风险告警 | 拒绝/有条件部分桶健康写 | 严禁返回损坏字节 |
| meta quorum丢失 | 目录不可线性化 | 强读拒绝或受控独立DR模式 | 拒绝 | 不独立调整目录 |

## 5. Scrub与Rebalance算法

`Scrub(job_id,manifest_index)`：读每Shard CRC→解码检查chunk SHA/AEAD→对可重建对象形成`RepairPlan`→在新generation写足保护布局→取得durable receipts→Raft CAS更新placement→同一Raft outbox写证据→旧分片延迟GC。发生乐观锁冲突重取manifest，禁止强行覆写新的位置。Rebalance同样先加后减、按故障域和水位限速并跟踪回滚点。

## 6. 空间、水位、I/O隔离

前台读写使用保留token；scrub/rebalance/迁移最低优先级且每zone限制带宽、IOPS、CPU预算。计费指标区分`logical_bytes`, `physical_bytes`, `reserved_bytes`, `inflight_multipart_bytes`, `retained_versions`, `orphans`, `pending_purge`，勿用逻辑容量÷1.5宣称可用容量（额外副本、WORM与故障预留会变化）。初值80%告警、90%写保护**仅为待基准测试的建议**。

## 7. 加密、时间与损坏证据

AEAD per-object/per-chunk nonce不重复，用KMS包裹数据加密密钥。nonce生成/保存及重试决策要求同一generation绝不复用相同(key,nonce)加密不同明文；生成新placement可保持原密文+codec并附完整校验。机器时钟不可作为唯一保留可信来源。

## 8. 运维证明项

每次发版执行Shard丢失/bitrot/慢盘/满盘/network partition/恢复/升级中断；收集manifest hash、receipt、health、读写判定与最终原文SHA。RTO和RPO必须分别按本地和跨站场景测量，不以EC数量推断SLA。
