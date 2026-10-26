# DD-MINIOS3-003 — S3协议与对象操作详细设计

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：S3 REST SigV4 PUT/GET/LIST/Multipart/版本/Lock


## 1. HTTP路由/Canonical解析

- Parse管道：TLS→保留`raw_target`及原始Host→SigV4 canonical request→鉴权→解析虚拟主机/路径式Bucket→Authorizer→应用handler→存储命令→XML响应。禁止在验证签名前做`path_clean`、Unicode NFC转换、重复编码规范化或删掉空Query。
- 支持AWS SigV4 Authorization和query签名；STREAMING-AWS4-HMAC-SHA256-PAYLOAD及trailers作为矩阵项单独开发/测试，未验证模式**显式拒绝**，绝不把签名检查关闭。
- `x-amz-date`窗口/credential scope/region/service、signed header排序、重复header规约及session token、body hash逐项验证；SignedHeaders失配拒绝。
- 公开Bucket-name在Endpoint上全局唯一；`/{bucket}/{key...}`可含原始斜杠字符，规范化仅按AWS canonical请求编码规则进行，不借用文件系统路径。

## 2. 业务操作处理表

| 操作 | 读/写目录 | 数据动作 | 正常返回 | 核心异常 |
|---|---|---|---|---|
| Create/Head/List/DeleteBucket | global group | 无 | 200/204/Location等按契约 | 409 BucketAlreadyExists/NotEmpty |
| PutObject | Prepare/Commit | stream→VerifiedReceipt | 200 + ETag/VersionId（适用） | 403/400/412/503 |
| GetObject/Head | ReadIndex读取 | Range→decrypt→verify | 200/206 / 200 | 404/416/503 |
| DeleteObject | ObjectHead transition | 保护判断后逻辑Mark | 204 + delete-marker attrs | 403受保护指定Version |
| CopyObject | 读源授权+写目标prepare | 服务端流式copy | 200 + XML copy result | 412/503/不支持能力 |
| ListObjectsV2 | 有序KV snapshot | 无 | 200 XML token/keys | InvalidToken/403 |
| ListObjectVersions | Version row序 | 无 | 200 XML历史版本 | InvalidArgument |
| MPU Initiate/UploadPart | upload元信息+part index | shard未发布数据 | XML UploadId / 200 ETag | 400 InvalidPart/BadDigest |
| MPU Complete/Abort | 单Commit或Abort状态 | 物理引用仲裁 | 200 XML / 204 | Conflict/NoSuchUpload |

不宣称所有AWS专属S3操作；遵循基线的release能力清单。ETag应按具体兼容配置生成，不得将其作为跨加密/MPU的内容摘要；`x-amz-checksum-*`及`Content-MD5`按宣称子集验证。

## 3. PUT操作详细时序与故障判定

1. 鉴权与授权；读取线性化head进行`If-Match/If-None-Match`检查并分配`operation_id`。`prepare_put`事务预留Quota、落入路由epoch并生成TTL票据。
2. 由`bounded_chunk_stream`持续读取数据，计算SHA-256和协议要求的CRC/Content-MD5，边编码/上传至目标持久层；禁止先接收全对象。
3. 按Backend capability获取`VerifiedReceipt`；Native要求全部所需6片按布局fsync+签名receipt；Federation核对provider响应+后续验证语义并记录证据。
4. `commit_put`再次检查票据和policy/route epoch、条件、quota、checksum，在**单一Raft命令**里新增ObjectVersion、更新ObjectHead、物理placement、审计Outbox、quota结算与幂等结果。
5. 读屏障确认后向HTTP客户端ACK；P3前失败仅orphan，P3后HTTP断开为“已提交/客户端未知”，重发前必须先查operation记录。

```text
OperationId + IntentHash -> PREPARED -> DATA_DURABLE -> COMMITTED -> ACK_SENT
                           \                     \
                            EXPIRED/ABANDONED       ACK_UNKNOWN (query result)
```

条件写在Commit点重复检查；`x-amz-content-sha256`覆盖值与实际字节不等则不提交。操作幂等只对**相同scope+intent+key**保持，冲突payload使用409，避免一个通用Idempotency-Key跨资源复用。

## 4. LIST与Continuation Token

E0/E1安全默认：单Bucket单Raft有序group。每次List取`read_index`、snapshot ID，按照Key字节排序迭代；prefix/delimiter/max-keys与common prefixes共享一个分页边界。`token = AEAD(version, bucket_id, tenant, filters_hash, snapshot_index, last_emitted_key, expiry, policy_epoch)`。服务端只接受当前授权scope+未到期snapshot，篡改或跨Bucket token返回InvalidArgument。snapshot retention必须覆盖分页TTL，若资源不足明确拒绝而不返回漏Key。

**跨range分片**：只有在跨组Barrier、快照保留和range迁移协议形式化证明并通过并发分页用例后方能开启；否则保持单Bucket单Group并明确资源吞吐上限。多页扫描中的新写入是否可见由同一个snapshot固定，直到token到期。

## 5. Versioning/Deletion/Object Lock

| Versioning状态 | PUT | DELETE 无VersionId | DELETE 指定VersionId |
|---|---|---|---|
| NeverEnabled | unversioned语义，后端能力必须匹配 | 无marker并按防护规则删除 | 必须按兼容矩阵拒绝不合法版本 |
| Enabled | 新non-null版本 | 创建新DeleteMarker，旧版保留 | 若锁/hold允许才删除特定版本 |
| Suspended | null-version语义 | 按AWS的null对象/marker规则 | null或显式版本分别按状态检查 |

上述表是**设计摘要**；Enabled↔Suspended与DeleteMarker、null版本覆盖/保护、re-enable等完整案例见ST协议矩阵，具体外部返回须通过真实SDK同AWS对照确定。Object Lock是每**Version**的Governance/Compliance/LegalHold，不是“桶被锁即不能写新版本”。策略不足/WORM无等价后端拒绝路由。

## 6. Multipart详细处理

`CreateMultipartUpload`生成`UploadId`(random/不可枚举) + `owner,route_epoch,created_at,checksum_algorithm,expires_at`；`UploadPart(number 1..10000)`把`part_no,etag,part_checksum,receipt,latest_attempt_seq`持久化到未发布目录；Complete先验证part列表排序/数量/最小大小边界、checksum一致、合法owner，再单Raft命令把全部parts构造成新ObjectVersion；成功后读取上传会话状态返回确定结果。Abort与Complete竞争时CAS锁定唯一胜者；重复Complete使用操作历史判定返回（不无条件承诺一定2xx）。上传未完成数据对GET/LIST不可见。清理仅回收不被Commited Manifest引用的part。

## 7. Range/下载语义

支持`bytes=start-end`、`bytes=-suffix`、`bytes=start-`、If-Range/If-Match等已声明场景；对可校验chunk执行精确映射、AEAD校验、CRC验证，拼接后返回精确字节流；错误范围416应包含正确`Content-Range`响应；**不允许返回先输出损坏字节再发现后段错误却仍以2xx完成**：对分块执行前置完整性检查或客户端结束时截断并记录传输失败，确保数据损坏告警而非“完整成功”的错误审计。

## 8. UT/IT关键矩阵

UT覆盖Canonical header排序、%2F与连续`//`、UTF8、时间漂移、SHA mismatch、part重复/乱序、token tamper、versioning状态机、失效Token、Range边界；IT采用aws-cli/boto3/JS/Go/Rust SDK真实签名、上游AWS及自建S3逐项比较XML/headers；ST验证20GiB流式、1000万Key分页要求（按测试硬件另合意）、数据故障/DR与锁保护。
