# DD-MINIOS3-009 — 多媒体版本差异、索引、派生和安全Worker

> v2.0 · 对应FR-MEDIA-001～010、FR-MV-010～025/033

## 1. 模型与输入

`MediaAsset(asset_id, repo_id, stable_identity)`与`AssetRevision(revision_id, parent_ids, manifest_id, schema_version)`是GitGit权威业务数据。MinIOS3仅保存内容和格式观察结果。`MediaObservation`及`DerivedArtifact`以不可变`source_manifest_id`, `adapter_version`, `settings_digest`为键，可重建，不能修改原件。

## 2. 扩展协议

Rust `MediaAdapter`（Trait见DD-016）支持 `inspect`, `preview`, `diff`, `merge`, `dependencies`五种可协商能力；合并不是任意格式默认能力。未知格式由`RawBinaryAdapter`提供hash/size/meta compare+冲突标记。

## 3. 各媒体详细策略

| 媒体 | Version Diff | Merge | 安全限制 |
|---|---|---|---|
| Image/PSD/TIFF | 原图叠加、可选层对比、色彩空间+阈值 | 仅带可证明结构操作的Adapter支持 | 解码限内存/像素数量 |
| Video MP4/MOV | 时间码差分、帧选择、音视频同步、代理关联 | 默认不自动合并 | FFmpeg隔离Worker、编解码许可检查 |
| Audio WAV/FLAC | 波形、频谱、区间标注 | 非线性工程工程格式需专用Adapter | 采样率归一须声明 |
| 3D GLB/FBX | 顶点/网格/材质/层级/动画关联对比 | 默认手工解决，不自动替换源 | 外部纹理/脚本资源禁止执行 |
| CAD/Unknown | Manifest hash、尺寸与元数据 | 强制显式二进制冲突 | 不解析未知恶意文件 |

## 4. Worker与缓存

Worker容器只获取只读、限时作用域的Source Manifest流；资源上限CPU/内存/FD/时间/磁盘/网络；输出独立DerivedArtifact UUID+SHA+来源；每项能力有`Supported/Conditional/Unsupported/Unverified`标识。任务死信重试不得重复生成公共可见Revision。

## 5. 源数据保护与一致性

预览失败/索引落后不修改GitGit提交；`SourceHash`始终与原始字节核对。搜索结果按repo/ref/asset权限二次过滤，显示EventWatermark与解析器版本。缓存须包含租户策略Epoch，撤权后不允许旧预览Token绕过检查。

## 6. 测试

UT：golden图像/音轨/模型和畸形格式处理；IT：Adapter超时和缓存撤权；ST：交互对比、几何差分和未知格式降级、商业素材许可约束。
