# TD-MINIOS3-UT-001 — Rust 単体試験設計書

> v2.0 / Not Run | 186条UT用例

## 1. 对象

GitGit：`gitgit-domain`, `gitgit-graph`, `gitgit-ref-store`, `gitgit-commit-coordinator`, `gitgit-media-diff`, `gitgit-workspace`, `gitgit-collaboration`；MinIOS3：`chunk-cas`, `manifest-catalog`, `ec-codec`, `s3-gateway`, `migration`, `authz`, `media-adapter-api`。

## 2. 用例设计

- Commit canonical编码与SHA稳定、Ref revision CAS抢占、DAG无环、rename资产ID稳定、冲突三方图模型。
- CDC min/avg/max参数稳定、offset连续、Hash碰撞输入验证、Chunk跨租户隔离、重复上传幂等、Manifest sealed状态。
- FakeClock模拟锁失效、令牌重放、策略Epoch改写；FakeReceipt模拟未fsync、placement epoch冲突；property tests要求无可见缺块Commit。
- MediaAdapter 对图片/audio/video/3D golden进行确定性断言，未知媒体Binary fallback；遇高风险媒体错误必须有上界而非panic。
- 原S3 SigV4/Multipart/WORM/Raft/EC/Provider Mock覆盖照原基线，但LFS相关UT仅检验可选互操作兼容。

## 3. 测试工具

`cargo test --workspace`, `proptest`, `cargo-fuzz`, `cargo-llvm-cov`, `cargo clippy`; ≥90%行/分支目标按源代码和范围签署，设计案例数不等于代码覆盖率。

## 4. 判定/输出

所有Case明确assert状态/错误/副作用、使用Fixture sha；完整步骤在`UT_完整案例明细_v2.0.md`，静态追踪在Excel。真实服务故障不由Mock UT代替。
