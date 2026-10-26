# IT-MINIOS3-002 — 結合テスト合同及故障注入设计

> 权威案例：`../../05_test_design/IT_完整案例明细_v2.0.md`。

## 1. 结合边界
GitGit RefStore↔CommitCoordinator；CommitCoordinator↔MinIOS3 ReceiptVerifier；Manifest↔ChunkStore；S3 Gateway↔Meta/Native/Federation；Migration↔RouteCatalog；Policy↔KMS；MediaWorker↔授权代理。

## 2. 重点场景
| ID | 结合关系 | 操作/故障 | 预期 |
|---|---|---|---|
| ITX-01 | GitGit↔MinIOS3 | 传完部分Chunk后Commit | 拒绝；没有可见缺块Ref |
| ITX-02 | GitGit↔MinIOS3 | receipt已签、RefCAS超时后重试 | 同幂等键返回唯一结果 |
| ITX-03 | Raft↔redb | leader掉电、log与snapshot换代 | ACK元数据持久且重放无重复提交 |
| ITX-04 | S3↔Backend | Multipart complete ACK丢失 | 查询唯一版本、不会生成损坏对象 |
| ITX-05 | Migration↔S3 | Cutover后新写入，然后回滚 | 反向同步尾差，未同步时拒绝回滚 |
| ITX-06 | GitGit↔GC | 正在创建Ref同时GC候选回收 | 重新核对reachability，不删新引用 |
| ITX-07 | Policy↔KMS | KMS掉线+密钥轮换 | Fail closed，不泄露密钥 |
| ITX-08 | MediaWorker↔Storage | 格式解析内存耗尽/外部URL | 隔离结束，原件可读取 |

## 3. IT测试环境
至少GitGit Core、MinIOS3 ControlPlane、Native Storage、可替换的S3测试后端、独立认证/KMS stub、故障注入代理和可检索的TraceID。Mock IT不能用于关闭真实分布式P0；真实故障需容器/VM层注入网络分区、kill-9、损盘、限流及时钟漂移。

## 4. 通过条件
每条案例证据记录镜像Digest、git hash、配置Epoch、测试数据SHA、故障起止时刻、观察到的Commit/Ref revision、S3 logical version和最终文件hash。不可复现实验不能宣称稳定通过。
