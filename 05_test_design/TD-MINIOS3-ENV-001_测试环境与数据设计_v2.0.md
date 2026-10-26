# TD-MINIOS3-ENV-001 — 测试环境 / テストデータ設計

> v2.0 | 无已搭建真实环境的声明

| 层 | E0 | E1/E2 |
|---|---|---|
| GitGit Core | 单机Rust版本图状态机，2用户2仓库 | GitGit多实例故障重启、分支数百万、受保护Tag/Review |
| MinIOS3 | Rust S3 Gateway/Manifest/Chunk Native单机和真实S3后端 | 3/5 Raft voter + 6 nodes K4M2 Native + 异地联邦S3 |
| 媒体样本 | 0B/1B、PNG/TIFF、WAV、MP4、GLB、未知binary、20GiB惰性流 | 专业Video/Audio/3D，PSD/FBX/游戏工程，100TB逻辑容量(真实度记录) |
| 失效样本 | Chunk被篡改、重复提交/断链、Ref CAS冲突、资源路径穿越 | 分区、KMS故障、AZ不可写、对象锁、GC/迁移并发、Worker恶意媒体 |
| 性能记录 | 原始SHA、seed、上传字节、CPU/RSS | 并发、p95、故障窗口、RPO/RTO、全恢复hash结果 |

所有媒体golden必须记录许可来源、标准hash、格式版本和预期差异结果；严禁使用未经许可的样本发布或商业分发。执行案例与设备配置签署后才能给ST Pass。
