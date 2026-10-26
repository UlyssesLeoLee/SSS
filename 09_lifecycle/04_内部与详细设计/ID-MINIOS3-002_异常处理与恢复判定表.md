# ID-MINIOS3-002 — 异常处理、重试与恢复判定

| 位置/错误 | 分类 | 对客户端 | 服务端恢复 | 不变量 |
|---|---|---|---|---|
| Chunk hash mismatch | DataIntegrity | 422/校验失败 | 拒绝Seal，隔离疑似块 | 不得生成有效Receipt |
| Manifest chunk缺失 | Precondition | 409/缺失列表 | 可从staging补传 | 已提交Ref不可引用 |
| Ref expected-old失配 | Conflict | 409/返回当前revision | 用户合并/重放比较 | 不得覆盖另一人的Ref |
| GitGit RefCAS ACK丢失 | Indeterminate | 202/查询transactionId | 查询事务表再恢复同结果 | 幂等键相同结果 |
| Raft Leader切换 | Retryable | 503 + Retry-After | 新任Leader自日志恢复 | 一次提交唯一生效 |
| Backend provider限流 | Throttle | 429/503（依协议） | 指数退避+全局背压 | 避免放大写入 |
| KMS不可达 | Denied/Unavailable | fail closed | 告警、短期重试 | 不得明文写入违规对象 |
| Retention拒绝 | Policy | 403+审计 | 无自动绕过 | WORM不可被管理员绕过 |
| 迁移尾差不足 | UnsafeCutover | 明确阻断 | 继续同步/回退 | 不丢ACK新版本 |
| MediaWorker崩溃 | DerivativeOnly | 预览降级 | 限额重试 | 原件不变 |
| GC证据不足 | SafeDefer | 管理告警 | 延期重算 | 不删保护内容 |

所有失败响应记录TraceID、OperationId、PolicyEpoch及一致性判定；审计中不得泄露访问密钥/令牌内容。错误映射与现有`DD-MINIOS3-012`协同维护。
