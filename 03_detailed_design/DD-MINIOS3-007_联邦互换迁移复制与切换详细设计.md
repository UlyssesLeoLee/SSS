# DD-MINIOS3-007 — 联邦互换迁移复制与切换详细设计

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：AWS S3/兼容Provider接入、Migrator、Replication、DR


## 1. Backend Trait契约

```rust
// 接口语义草案：最终代码需结合SDK版本进行签名和实现。
trait StorageBackend {
  async fn probe(&self) -> Result<ObservedCapabilities, BackendError>;
  async fn put_immutable(&self, intent: PhysicalIntent, stream: ByteStream) -> Result<VerifiedReceipt, BackendError>;
  async fn get_range(&self, placement: &PhysicalPlacement, range: Range) -> Result<ByteStream, BackendError>;
  async fn list_versions(&self, cursor: Option<OpaqueCursor>) -> Result<VersionPage, BackendError>;
  async fn delete_version_if_allowed(&self, intent: PurgeIntent) -> Result<(), BackendError>;
}
```

能力状态`Verified / Conditional / Unsupported / Unverified`按 `backend+API+feature+SDK+release`保存检测证据；未知高风险能力=WORM、Versioning、KMS、Checksum/Retention时Fail-Closed。后端凭据来自SecretRef；禁用用户绕过网关写入或至少通过provider IAM deny policy强制限制，不可证明单主则不能宣称无损迁移。

## 2. PhysicalPlacement映射

Public `(tenant,bucket,key,logical_version_id)`是唯一对外版本寻址键；PhysicalPlacement可能同时保存 Source 和 Target（迁移期间）两份，并以`epoch + generation + verify_digest`区分。Provider的VersionId只写在物理映射，不回写公开LogicalVersionId。`metadata/tags/delete-marker/retention/legal_hold/encryption-ref`有字段级语义对照；缺功能时生成BlockedReason，而非伪造成功。

## 3. Migration状态机与作业记录

```text
DRAFT -> CAPABILITY_CHECK -> DRY_RUN -> AWAIT_APPROVAL -> INVENTORY
  -> BULK_COPY -> DELTA_CATCHUP -> VERIFY -> WRITE_FENCE -> FINAL_DELTA
  -> CUTOVER_CAS -> OBSERVE -> RETIRE_APPROVAL -> FINISHED

任意校验/法规失败 -> BLOCKED（只读报告）
WRITE_FENCE之前失败 -> REVERT_TO_SOURCE（旧源仍主写）
CUTOVER_CAS之后失败 -> REVERSE_CATCHUP -> FENCE_TARGET -> CUTOVER_BACK_CAS
                         \ 追平不可达 -> STAY_TARGET/READ_ONLY（禁止盲回旧源）
```

JobLedger字段`job_id, owner, source, target, bucket_id, source_epoch,target_epoch, phase, cursor, inventory_digest, delta_watermark, approval_signatures, lease_owner,lease_expiry, src_manifest_hash,tgt_manifest_hash, blockers, cancel_policy`。Checkpoint写Raft后才发起下一个batch；`operation_id`贯穿每件复制，Retry需先判目标Version映射是否已提交。

## 4. 差量捕获和写栅栏

Source authoritative writer期间所有业务提交带`route_epoch`并进入有序Outbox；目标shadow仅复制，不接客户端写。Inventory按所有Version（含DeleteMarker、null版本、保留/Tag/metadata/ACL适用项）扫描，记录快照与cursor；增量从LogIndex同步，乱序由Version序列校验并重放，不允许“最后到达覆盖”。切换时关旧epoch新写入口（Fence），等待所有inflight commit resolved，再推进final delta，核查完整性和WORM，执行一个全局Bucket route CAS。旧源只读保留直到观察期结束及两人审批。

## 5. 校验边界

- 对所有对象版本：**原始SHA-256+byte length 100%检查**（满足NFR-MIG-001），另逐项对比Version层次/顺序、DeleteMarker、tag、user metadata、retention、legalHold、ACL授权结果及KMS解密能力。
- MPU对象ETag不是跨后端sha；不能只比较ETag。
- 若源后端不提供可可靠取得的旧版本/流，记录未迁移条目并阻断目标生产Cutover；不得将丢失历史隐藏在成功率百分比内。
- 100万对象E0规模可能在部署预算内难以实现；**需求CR-007尚未签署**，故作为阻断验收待实施，不悄然降为10万。

## 6. 回滚与删除

切换前回滚只解除Fence、恢复Source writer；切换后已有新确认PUT，则回滚必须从Target向Source逆向补齐全部新版本与ACL/Lock语义，再Fence Target、比对两个watermark并CAS回Source。逆向复制失败则继续保持Target主写或受控只读，**不得立即恢复Source写入造成数据分叉**。老源退役只能在WORM延长保护/审计和出口备份证据签收后提交。

## 7. Replication与读容灾

Async replication消费Raft outbox日志，按`bucket,key,version_seq`去重、顺序化，记录`synced_log_index, lag_seconds, RPO_estimate, checksum_verified`。跨站read failover显式标记RPO窗口；物理已存在而目录未同步的对象不得伪报为已一致。DR提升新主需外部fencing证据和元数据恢复完整性快照；跨站RPO≤15min / RTO≤60min均为**待验证的NFR草案**。

## 8. QoS和异常

每Backend独立bounded queue、circuit breaker、Retry-After、exp backoff jitter、租户配额；强制Region/residency策略在每次复制提交前重验。分区、429、503、版本列表空洞、回包成功但链路断开、token过期、WORM不匹配、KMS无权限都属于必须阻断或补偿的分支。审计迁移管理员、两次批准、每个hash mismatch及路由CAS事件。
