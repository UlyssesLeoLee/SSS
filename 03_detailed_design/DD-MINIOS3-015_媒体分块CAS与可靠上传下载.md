# DD-MINIOS3-015 — CDC分块CAS/Manifest与可靠传输

> v2.0 | 模块权威 MinIOS3 | 对应FR-MV-003～009、FR-MV-027～030

## 1. Chunk模型

`ChunkKey(tenant_encryption_domain, algorithm_version, blake3_digest, size)`，`ContentManifest(version, original_sha256, file_size, ordered_chunk_refs, codec_policy, content_type_hint)`，其中ChunkRef为(offset,len,key,checksum)。Manifest字节采用规范编码，以SHA-256求ManifestId；BLAKE3为内部快速完整性检查，原始SHA-256才是完整文件一致性权威。

## 2. FastCDC参数

MVP建议范围min=1MiB、avg=4MiB、max=16MiB（均为**候选默认值**，需按媒体类型profiling）；超大压缩视频既可能无法产生有效重用，也可能效率低于固定块，按策略切换`FixedChunker`。Chunker版本和参数必须进入Manifest，重放不得使用运行时默认值替代历史。

## 3. 上传Session状态机

`Created → Receiving → AllChunksVerified → ManifestSealed → Referenced/Expired`。一次上传查询Chunk存在性仅返回当前租户授权范围内的命中；缺块PUT采用幂等`chunk_key + upload_session + ordinal`，对重复字节比较hash和长度；不一致拒绝。收到`ManifestSeal`前校验Chunk连续无gap无overlap、sum(size)=original size、物理Receipt与源SHA。

## 4. 下载算法

Range重组读取Manifest的必要Chunk，在固定上界内存队列中异步流式解密、校验、切片输出；客户端可以只要求所需Chunk。任何校验失配返回完整性错误并隔离副本，不能返回静默损坏内容。

## 5. 数据可靠性

Chunk ACK需在Native达到纠删码的数据保护门禁+元数据可见性或Federation完成目标Durable约定后生成。Meta Raft日志写入Manifest+Receipt，保留写入epoch；失去写入多数派或故障域条件时拒绝新Manifest提交，不能以本地缓存收据充数。

## 6. 并发与回收

Manifest/Chunk对象不可变，引用计数仅为加速指标，物理删除依赖全局reachability快照/GC epoch/保留策略双确认。上传Session保留必须具有有界TTL和对准备中的GitGit事务保护。

## 7. 测试

CDC对前部插入、byte-offset、参数升级、恢复、跨租户侧信道与缓存；UT性质测试+IT断点/重复上传+ST20GiB/TB构造媒体、故障注入及RSS曲线。
