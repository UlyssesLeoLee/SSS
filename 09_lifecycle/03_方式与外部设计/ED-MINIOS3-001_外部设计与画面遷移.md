# ED-MINIOS3-001 — 外部设计（外部設計）画面与用户交互规格

## 1. 画面总览及编号
| 画面ID | 页面 | 信息和操作 | 来源要件 |
|---|---|---|---|
| UI-01 | Repository Dashboard | Branch/Commit、媒体总量、最近变更、风险提示 | FR-GG-* |
| UI-02 | Media Assets | 筛选/搜索、缩略图、资产版本及源校验摘要 | FR-MV-* |
| UI-03 | Media Diff | 左右/叠加对比、时间码、3D视角；不可解析则降级 | FR-MEDIA-005 |
| UI-04 | Review & Merge | MergeBase、冲突列表、锁状态、裁决和审批 | FR-GG-* |
| UI-05 | Storage Explorer | Bucket/Key、逻辑VersionId、Retention、实际后端状态 | FR-S3-* |
| UI-06 | Migration Console | Dry-run、进度、校验错误、epoch和切换/回退向导 | FR-XCHG-* |
| UI-07 | Ops & Security | Cluster health、授权审计、KMS、故障域告警 | FR-OPS-* |

## 2. 屏幕共通动作约定
搜索、分页、刷新保持选择态；版本链接均包含repo/commit/assetRevision的稳定标识。危险操作（delete、GC、cutover、解除锁）至少二次确认+权限校验并产生审计；WORM和跨租户违规操作不可仅靠UI隐藏按钮保护。

## 3. Media Diff交互状态
`Loading → Ready | FallbackHashOnly | Unauthorized | DecodeFailure`。任何解码失败仅影响派生展示，原始下载仍独立授权。用户可以在两个Commit之间移动，差异区域和来源hash可检查；插件能力为可扩展而非无条件支持。

## 4. 错误与可访问性
统一显示error_code、request_id、可行动建议、是否可安全重试；不显示KMS密钥、内部bucket凭据或对象完整路径。键盘可操作，焦点可见，非仅颜色表达冲突/锁定；传输进度保留上次失败诊断。

## 5. 浏览器和前端
Vue3/TypeScript/Vite和CSS变量；图像Canvas、3D Three.js通过独立预览组件；大媒体不得整个载入浏览器内存；只通过短时授权Range流访问。兼容性随浏览器测试矩阵单独冻结。
