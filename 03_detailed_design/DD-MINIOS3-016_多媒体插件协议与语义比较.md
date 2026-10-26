# DD-MINIOS3-016 — MediaAdapter Trait、媒体语义差分及安全性

> v2.0 | 模块负责人 GitGit Media / MinIOS3 MediaWorker

## 1. Adapter API设计

```rust
// 逻辑签名示意；I/O统一异步Streaming，并不保证可直接编译
#[async_trait]
trait MediaAdapter: Send + Sync {
    fn descriptor(&self) -> MediaAdapterDescriptor;
    async fn inspect(&self, source: ReadOnlyManifestStream, limits: Limits)
        -> Result<MediaObservation, AdapterError>;
    async fn diff(&self, left: RevisionView, right: RevisionView, opts: DiffOptions)
        -> Result<MediaDiff, AdapterError>;
    async fn preview(&self, source: ReadOnlyManifestStream, opts: PreviewOptions)
        -> Result<DerivedArtifact, AdapterError>;
    async fn merge(&self, base: RevisionView, left: RevisionView, right: RevisionView)
        -> Result<MergeResult, UnsupportedOrConflict>;
}
```

## 2. 能力协商矩阵

`MediaAdapterDescriptor`含`adapter_id,version,formats,mime_signatures,capabilities,determinism,security_profile,license_artifact`。`Unsupported`与`Unverified`不得自动转成Supported。RawBinaryAdapter为所有未知媒体的最低保真回退，提供字节hash差异+人工冲突。

## 3. 语义差异的数据模式

`MediaDiff`可包含`StructuralChange`, `TimeRangeChange`, `PixelRegionChange`, `WaveSegmentChange`, `MeshChange`等，但所有结果均须带`source_revision_left/right`, `adapter_version`, `options_digest`, `confidence`及`provenance`。媒体比较只是辅助审查，GitGit版本依据仍是Manifest和Commit。

## 4. 安全隔离

Worker独立进程/容器; CPU/RAM/time/output/FD硬限制、受限网络、只读源流；不装载密钥和宿主挂载；外部FFmpeg/编解码器许可证需按固定构建审查；Rust WASM插件可作为未来方案，但不可直接当作已实现沙盒。

## 5. 兼容和升级

Adapter升级保持既有observations历史版本；缓存键包含源Manifest与参数+Adapter版本+租户政策epoch；撤权后预览不可绕开授权。

## 6. 验收与测试

每媒体格式最少1个标准正例+5个恶意/边界样本；semantic golden表应验证色彩空间/时间码/帧率/采样率/模型拓扑，任何未实现格式标Conditional/Unsupported并展示原件下载路径。
