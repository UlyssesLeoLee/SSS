# DD-MINIOS3-001 — 详细设计总则与工程架构

> **v2.0统一约束：** GitGit Core原生版本引擎对Commit/Ref/AssetRevision拥有权威；MinIOS3负责Manifest/Chunk、S3和存储。凡旧段落出现LFS/外部Git集成语义仅用于可选兼容，不构成原生业务主路径。详细见DD-014～019。

> 版本 1.0-draft｜日期 2026-10-26｜状态：详细设计草案（未实施／未测试）
>
> 输入：SRS-MINIOS3-001 v2.0；BD-MINIOS3-001～011 v2.0；对抗式审查 REV-001～028。与 IPA 共通フレーム/非機能要求グレード的文档管理方式对齐，但不是 IPA 认证。
>
> 适用范围：E0、E1、E2全局


## 1. 设计基线与冻结边界

- 一级真实来源：SRS功能104条、非功能32条、业务用例12条；不得修改其原始编号。二级来源为Rust基本设计11分册、OpenAPI草案、兼容性清单和REV审查记录。
- 详细设计不擅自批准CR-001～008；任何对S3、可写性、WORM、迁移回滚或Git Hook的承诺变化，须需求所有者签署CR才能变为发布承诺。
- 设计采用**单一公开S3 Endpoint + 主数据Raft目录 + Storage Backend抽象 + Git LFS兼容独立CAS授权**。`Mixed`仅表示不同Bucket按后端路由并存，不允许同Bucket多活主写。
- 默认E0联邦网关方案：后端被隔离为唯一主写；单Raft范围仅用于功能验证且明确无生产HA。E1原生集群启用Raft和EC；E2非S3适配器与高阶媒体。
- 核心服务器代码Rust；Web UI不强制Rust；第三方KMS、IdP、Raft数据库底层OS并不属于Rust自研范围。工具依赖版本在Cargo.lock/BOM冻结时确定。

## 2. Runtime进程与实例边界

| Binary / crate | 端口（示例；可配置） | 对外服务 | 可持久化目录 | 扩展方式 | 故障隔离边界 |
|---|---|---|---|---|---|
| minios3-gateway | 9000/TLS | S3 REST/XML、Git LFS兼容 Batch与传输 | 禁止持久原始对象 | 水平扩展 | HTTP请求预算/每租户并发 |
| minios3-admin | 9443/TLS | 管理REST JSON、健康与审批 | 不存密钥 | 水平扩展 | 管理流量独立pool |
| minios3-meta | 9444/mTLS | Meta command/read/snapshot、Raft | WAL、snapshot、state KV | 3/5 voter | term/read-index quorum |
| minios3-storage | 9445/mTLS | shard Put/Get/Scrub | append-only shard + fsync journal | 域感知增加节点 | disk/node/zone |
| minios3-worker | 无公网端口 | 复制、迁移、GC、预览、audit outbox | Checkpoint位于Raft | worker lease | queue与资源预算 |
| minios3ctl | 无监听 | Admin API客户端 | 用户配置/证书引用 | 独立发行 | 凭据最小权限 |

端口为**配置例**，不是既有服务已开放事实；业务逻辑仅通过 `crates/` 内的Domain接口复用。服务间默认mTLS；公共API独立TLS证书。

## 3. Crate依赖方向（禁止逆向依赖）

```text
protocol-s3  protocol-lfs  admin-contract
       \       |       /
        gateway-application
         |             \
  authz / domain / observability
         |       \
       catalog   git-integration   migration
         |         |                 |
         +---- metadata-command ----+
                  |
          storage-api (trait)
             /          \
      storage-native   storage-s3
```

约束：`domain` 不得依赖网络与具体KV；`protocol-*`不得直连数据节点；`migration`不可绕过catalog直接变更published/latest；`web/`只能通过Admin API。物理删除统一走保留与引用复核仲裁器，不允许每个worker自行判定。

## 4. 标识与不可变性

| 名称 | 表示形式 | 归属 | 能否变化 | 注意 |
|---|---|---|---|---|
| tenant_id/project_id/bucket_id | UUID/ULID语义强类型 | MinIOS3 global catalog | 不复用 | 外部Bucket名称单Endpoint全局唯一 |
| key | 原始请求解码后合法字节序列 | Bucket | 同Key可新版本 | 不进行文件系统规范化 |
| logical_version_id | 随提交序生成的不可猜稳定ID | MinIOS3 | 不变 | 映射不同provider版本 |
| lfs_oid | lowercase 64 hex SHA-256 | Git LFS兼容协议 | 内容不可变 | 不是Git提交SHA |
| git_commit_oid | GitGit Core提供（哈希算法由Git仓库决定） | GitGit Core | 不变 | 不从LFS兼容OID推断 |
| backend_version_id | Provider提供的opaque ID | 物理placement | 可因迁移改变 | 只存在私有映射 |
| route_epoch / policy_epoch | 单调增整数 | Global metadata | 严格递增 | 使用CAS/fencing |
| operation_id / event_id | 不可猜随机ID | 端到端命令/消息 | 同请求保持 | 防重复提交与事件 |

## 5. 端到端不变量与所有权

- I-01：2xx对象版本具备可校验持久数据收据、已提交目录以及审计outbox；任一不成立拒绝ACK。
- I-02：读同一Bucket Key/Version必须先授权，再解析逻辑版本到物理placement，最后校验字节；损坏不得以成功200/206返回。
- I-03：上传存储与Git Ref更新两个系统不可实现单ACID事务；必须Hook预检查、Git原子Ref CAS、事件通知和定期全量对账，索引未完成不得误GC。
- I-04：Object Lock遵循单版本语义，不得在迁移时缩短Retention；根管理员物理破坏威胁无法仅靠本软件证明合规。
- I-05：`ListObjectsV2` 的强一致作用域是单Bucket单元数据Raft group；如分片群之间无形式化跨组snapshot契约，禁止打开同Bucket多group模式。
- I-06：路由切换后发生新ACK写时，直接回到旧源是**禁止**的；必须逆向追平+重新Fence，或者阻断并保留现行主源。

## 6. 共通Rust约束

1. 禁止`unwrap()`/`expect()`处理来自网络或磁盘的不可控输入；内部panic视为错误隔离/Sev触发。
2. 每次I/O操作必须具deadline与CancellationToken；传输流内存上界由chunk池、`Semaphore`、有界mpsc共同约束。
3. 错误统一用域错误枚举+稳定外部映射，但S3必须XML错误码、LFS按标准JSON/HTTP返回、Admin用JSON Problem；无跨协议JSON包装。
4. 所有写命令有结构化`op_id, actor, tenant, bucket, route_epoch, policy_epoch, trace_id`，日志不含凭据或密钥材料。
5. 配置使用明确schema版本，敏感键仅secretRef；循环依赖/未支持配置启动前拒绝，未知安全键不得静默忽略。
6. 数据迁移、存储修复、Raft迁移使用Forward+Compatibility窗口；状态不可逆改写须提前保存可恢复快照。

## 7. 实施顺序、门禁与例外

执行顺序：**冻结DD接口契约 → 编写生产代码 → 编写UT/IT/ST并验证**。本包先行提供测试设计，供代码开发按测试设计驱动实现。门禁编号 G0需求合意、G1协议/安全契约、G2单元/性质验证、G3真实后端与Git联调、G4故障/性能/DR验收、G5发行审查。

未关闭的 `REV-001..012` 属安全/数据完整性重大风险；DDL可实现性已设计≠已验证，不得输出“企业级已达标”。

## 8. 详细设计文档导航

DD-002 Rust内部模块；DD-003 S3协议；DD-004元数据；DD-005 Native；DD-006 Git；DD-007迁移；DD-008安全；DD-009媒体；DD-010任务运维；DD-011部署DR；DD-012协议错误/契约；DD-013逐需求映射。各流程相关Mermaid在 `../06_diagrams/`。
