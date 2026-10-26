# MAINT-MINIOS3-001 — 保守・维护・升级设计

## 生命周期
Rust工具链与依赖更新采用受控版本；Cargo.lock、SBOM、source provenance与许可证扫描不能缺失。依赖大版本升级必须运行原有UT/IT/ST合同回归并记录向后兼容破坏。

## Schema升级
所有持久化结构使用显式schemaVersion；变更路径为`OldRead → NewRead/Write → DualRead验证 → Finalize`，禁止就地修改不可变Commit或Manifest。数据迁移支持暂停/恢复/回滚性评估；不保证所有schema都能自动回退。

## GitGit协议版本
Client与Server对协议版本及capabilities协商；旧客户端不理解的MediaAdapter能力不静默执行；未知Commit字段解析规则应与规范化摘要一致且不会破坏ID稳定性。

## S3兼容演进
发布版本带compatibility manifest和实际AWS SDK语种/版本回归结果。新API支持级别至少分为Supported、Partial、Unsupported、RequiresBackendCapability，不允许营销“100% S3”而未验证全部接口。

## 不可变数据与GC保守策略
引擎升级前后对旧Commit golden vector做hash回归；在reachability schema不兼容或事件重放不完备时暂停物理GC，防止升级导致历史媒体误删。
