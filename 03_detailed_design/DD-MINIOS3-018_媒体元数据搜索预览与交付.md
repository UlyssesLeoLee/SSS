# DD-MINIOS3-018 — Asset Index / Preview / S3版本互换

> v2.0 | 关联FR-MV-020～034及原FR-MEDIA-001～010

## 1. 搜索与检索权威

搜索索引属于可再生成视图，以已确认GitGit Commit/AssetRevision的事务Outbox重建。结果必须同时按租户、仓库、分支、Asset ACL过滤，`indexed_through_seq`展示索引滞后；不能使索引成为Ref发布必要写入路径。

## 2. 预览和代理对象

`DerivedArtifact {derived_id, source_revision_id, source_manifest_id, adapter, adapter_version, params_hash, derived_manifest_id, status}`；生成失败不影响原件; 媒体playback URL默认经网关进行每请求AuthZ/retention规则检查，禁止未经审查直链云后端成为撤权漏洞。

## 3. S3互换

GitGit Revision与S3 LogicalVersion及各Provider `physical_version_id`明确分别存储。针对源+目标所有仍可达Manifest/Chunk进行完整hash校验，切换按RouteEpoch栅栏；切换后新ACK写入没有反向追平，不得直接回滚到旧源。

## 4. 安全审计/许可

原始版权元数据由AssetRevision携带版本；导出操作必须包含Manifest/Chunk SHA、GitGit树与Commit签名，若第三方素材授权不满足分发范围则FailClosed。预览Worker的OSS许可证审查是构建链发布Gate，而非只记录工具名称。

## 5. 测试

IT媒体索引watermark延迟、撤权缓存、跨S3完整历史读；ST S3迁移后导出/签名链校验及百万资产导入、异常行为回退。
