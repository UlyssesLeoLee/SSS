# ST 完整测试案例明细 — GitGit + MinIOS3 v2.0

> 共 249条设计案例 | Not Run | 所有Case ID唯一；旧案例沿用编号，新业务主链路按原生GitGit验收。

## 001 ST-FR-CORE-001 — 独立部署-系统业务验收

- **对应要件：** FR-CORE-001 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 验证新要求：断开公网后，GitGit原生提交、版本回退、媒体分块上传/下载、MinIOS3 PUT/GET/LIST和管理可正常完成；禁用LFS适配仍通过。；关闭Git LFS兼容适配时原生媒体版本提交仍必须可用。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 002 ST-FR-CORE-002 — Native 与 Federation 运行模式-系统业务验收

- **对应要件：** FR-CORE-002 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：两种部署模式均通过 S3 核心兼容测试；模式切换不得改变现有对象内容。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 003 ST-FR-CORE-003 — 租户层级-系统业务验收

- **对应要件：** FR-CORE-003 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：租户 A 凭据无法枚举或读取租户 B 的任何资源；隔离自动测试100%通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 004 ST-FR-CORE-004 — 租户资源计量和配额-系统业务验收

- **对应要件：** FR-CORE-004 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：超出硬配额返回明确错误，管理员可追踪来源；计量误差可核对。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 005 ST-FR-CORE-005 — 存储命名空间和资源映射-系统业务验收

- **对应要件：** FR-CORE-005 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：迁移后使用原端点、原 Bucket/Key 可以正确读取原版本。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 006 ST-FR-CORE-006 — 配置版本与回滚-系统业务验收

- **对应要件：** FR-CORE-006 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：错误路由配置可恢复上一生效版本，不改变已确认对象。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 007 ST-FR-CORE-007 — 多区域和数据驻留策略-系统业务验收

- **对应要件：** FR-CORE-007 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：禁止区域间转移时，对应迁移/复制任务无法启动并写入审计记录。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 008 ST-FR-CORE-008 — 能力发现接口-系统业务验收

- **对应要件：** FR-CORE-008 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 在离线/多租户/不同运行形态生产模拟环境按需求创建、读写、切换配置；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 服务在约束条件内正确运行；公网依赖单独标记不可用，不伪承诺；逐项验收：API和UI可取得对应版本的能力清单；不支持功能明确返回非兼容错误。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 009 ST-FR-S3-001 — Bucket 生命周期-系统业务验收

- **对应要件：** FR-S3-001 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：创建、列举、空桶删除、权限错误及地域配置测试通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 010 ST-FR-S3-002 — 标准对象读写-系统业务验收

- **对应要件：** FR-S3-002 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：主流 S3 SDK 的上传、下载、覆盖、条件请求符合兼容基线。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 011 ST-FR-S3-003 — 对象列举-系统业务验收

- **对应要件：** FR-S3-003 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 预置 Key [a,a/1,a//b,汉字,emoji], prefix/delimiter筛选；同时插入100新key并用同一快照token翻页；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 每页最多max-keys，字节排序无重复无遗漏；篡改token/跨tenant token拒绝；对照原要件验收：千万级键空间分页不漏不重；与提交确认的写入符合指定一致性。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** REV-010:跨range强LIST未证明前只测单Bucket单Group

## 012 ST-FR-S3-004 — 分段上传-系统业务验收

- **对应要件：** FR-S3-004 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) CreateMultipart；乱序上传Part 3/1/2；并发Complete与Abort；对已提交session重试并断开HTTP；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 只有一次ObjectVersion publish，parts排序准确；未发布part对GET不可见；对照原要件验收：分段乱序、断线续传、重复Complete、Abort及并发重试通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 013 ST-FR-S3-005 — 版本控制-系统业务验收

- **对应要件：** FR-S3-005 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 在NeverEnabled→Enabled→Suspended→Enabled下依次PUT/DELETE含显式VersionId、null及DeleteMarker；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** VersionId稳定，旧版合法GET，锁定版本不可删除，DeleteMarker符合兼容矩阵；对照原要件验收：覆盖与删除后，历史版本可依授权恢复，版本标识稳定且唯一。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 014 ST-FR-S3-006 — 签名鉴权-系统业务验收

- **对应要件：** FR-S3-006 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 准备已知AWS SigV4 canonical向量，依次修改Host、%2F、SignedHeaders、X-Amz-Date、payload bytes；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 签名任何参与字段被改即403，TLS代理未修改原始Host/Path时合法签名成功；对照原要件验收：官方AWS SDK认证场景及坏签名、过期时间戳均有正确响应。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 015 ST-FR-S3-007 — 临时预签名-系统业务验收

- **对应要件：** FR-S3-007 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：过期、越权、撤销后无法访问；范围约束与审计关联正确。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 016 ST-FR-S3-008 — HTTP Range 与流式传输-系统业务验收

- **对应要件：** FR-S3-008 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：对≥20GiB对象进行Range读取，不整体加载内存，返回正确206与校验。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 017 ST-FR-S3-009 — 对象属性及标签-系统业务验收

- **对应要件：** FR-S3-009 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：元数据/标签在复制迁移后按策略完整保留。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 018 ST-FR-S3-010 — 分层策略-系统业务验收

- **对应要件：** FR-S3-010 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：默认拒绝、多租户隔离、策略冲突和SDK常见操作矩阵通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 019 ST-FR-S3-011 — 生命周期管理-系统业务验收

- **对应要件：** FR-S3-011 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：TTL到期任务正确执行；被保留/保护的版本不被误删。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 020 ST-FR-S3-012 — Object Lock-系统业务验收

- **对应要件：** FR-S3-012 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 旧Version设Governance和Compliance，试Governance绕过、有无权限、缩短Compliance、LegalHold解除、时钟回拨；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Compliance始终拒绝提前物理删除；新Version/合法DeleteMarker按协议允许；对照原要件验收：WORM对覆盖、版本删除、撤销保留、时钟回拨等攻击测试拒绝；独立审计完成。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 021 ST-FR-S3-013 — 事件通知-系统业务验收

- **对应要件：** FR-S3-013 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：消费者重复收到事件也能幂等处理；失败可重放。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 022 ST-FR-S3-014 — SSE/KMS兼容-系统业务验收

- **对应要件：** FR-S3-014 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：使用配置KMS读写正常，密钥拒绝/轮换行为受测。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 023 ST-FR-S3-015 — Object Checksum-系统业务验收

- **对应要件：** FR-S3-015 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：篡改块校验失败；迁移前后SHA-256一致。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 024 ST-FR-S3-016 — S3 Batch 操作-系统业务验收

- **对应要件：** FR-S3-016 | 阶段 E2 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：10万对象批量任务中断重启不丢状态、无重复破坏性变更。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 025 ST-FR-S3-017 — S3 兼容性清单-系统业务验收

- **对应要件：** FR-S3-017 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：发布版本附机器可读兼容清单，并对宣称支持的API自动回归。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 026 ST-FR-S3-018 — 虚拟主机与路径风格-系统业务验收

- **对应要件：** FR-S3-018 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：两种地址形式在合法桶名、特殊字符、代理TLS下通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 027 ST-FR-S3-019 — 对象幂等与并发条件-系统业务验收

- **对应要件：** FR-S3-019 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 按S3真实SDK矩阵对每操作进行正确/错误/并发/超时/大对象路径全链路演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 校验真实HTTP/XML/Header和原件SHA，版本/锁语义不丢；逐项验收：重试不会生成损坏版本；并发相同键操作呈现可解释顺序。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 028 ST-FR-GIT-001 — [v2修订] Git LFS Batch API - ST验证

- **对应要件：** FR-GIT-001 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：LFS兼容客户端传输与SHA校验通过；禁用LFS兼容服务不影响GitGit原生E0媒体版本链路。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 029 ST-FR-GIT-002 — [v2修订] Git LFS Locking API - ST验证

- **对应要件：** FR-GIT-002 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：GitGit锁与LFS锁查询一致；未启用LFS时原生协作锁仍有效。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 030 ST-FR-GIT-003 — [v2修订] 标准Git独立性 - ST验证

- **对应要件：** FR-GIT-003 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：关闭标准Git互操作插件后GitGit原生版本管理仍能提交、分支、比较和恢复。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 031 ST-FR-GIT-004 — [v2修订] 内容寻址 - ST验证

- **对应要件：** FR-GIT-004 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：同字节同策略域复用且不改变原件；租户隔离，读取重新校验。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 032 ST-FR-GIT-005 — [v2修订] 引用完整性检查 - ST验证

- **对应要件：** FR-GIT-005 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：缺块、旧epoch、非法ACL或过期票据均拒绝提交；旧Ref保持不变。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 033 ST-FR-GIT-006 — [v2修订] 版本关联索引 - ST验证

- **对应要件：** FR-GIT-006 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：Commit→资产→内容块→物理存放可逆向追踪且hash验证一致。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 034 ST-FR-GIT-007 — [v2修订] Git分支/Tag与媒体历史 - ST验证

- **对应要件：** FR-GIT-007 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：创建、切换、比较、回退GitGit分支正确还原媒体内容；S3版本ID差异不影响历史。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 035 ST-FR-GIT-008 — [v2修订] Git身份继承 - ST验证

- **对应要件：** FR-GIT-008 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：撤销GitGit权限后新的原生读取及签名URL请求被拒；跨租户不可枚举CAS。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 036 ST-FR-GIT-009 — [v2修订] Git事件接入 - ST验证

- **对应要件：** FR-GIT-009 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：重复、乱序、遗漏后通过快照watermark对账；GC对未证完整可达性拒删。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 037 ST-FR-GIT-010 — [v2修订] 媒体垃圾回收安全 - ST验证

- **对应要件：** FR-GIT-010 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：在并发新引用、迁移、快照与合法保留期间零误删。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 038 ST-FR-GIT-011 — [v2修订] 离线和部分克隆 - ST验证

- **对应要件：** FR-GIT-011 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：离线提交后重连按冲突策略重新校验；只下载请求Chunk并保持SHA一致。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 039 ST-FR-GIT-012 — 仓库配额和限流-系统业务验收

- **对应要件：** FR-GIT-012 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff；2) 真实仓库进行Push/Force-Push/Tag/Fork/锁冲突/删除后恢复/全量Ref对账；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 提交永不引用缺失OID；Git历史可checkout且无误GC；逐项验收：超配额上传在写入前可预测拒绝，已存在历史不被误删。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 040 ST-FR-GIT-013 — [v2修订] 提交一致性补偿 - ST验证

- **对应要件：** FR-GIT-013 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：断电、超时、重复请求不会产生可见缺块提交；补偿可恢复且GC保护。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 041 ST-FR-GIT-014 — [v2修订] 现有Git迁移 - ST验证

- **对应要件：** FR-GIT-014 | 阶段 E2 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：导入后保留历史关系与内容摘要且输出无法映射特性的差异报告。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 042 ST-FR-MEDIA-001 — 多媒体资产分类-系统业务验收

- **对应要件：** FR-MEDIA-001 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：至少PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP完成端到端验证。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 043 ST-FR-MEDIA-002 — 资产元数据-系统业务验收

- **对应要件：** FR-MEDIA-002 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：元数据与Git Commit及OID可以双向检索。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 044 ST-FR-MEDIA-003 — 内容零改写-系统业务验收

- **对应要件：** FR-MEDIA-003 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：下载SHA-256与原上传文件相同。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 045 ST-FR-MEDIA-004 — 异步预览-系统业务验收

- **对应要件：** FR-MEDIA-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：预览出错时下载与版本读取不受影响，且有失败原因。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 046 ST-FR-MEDIA-005 — [v2修订] 版本差异辅助 - ST验证

- **对应要件：** FR-MEDIA-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：通过媒体专用golden fixture识别变化，无法识别时回退hash/size而不宣称语义相等。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 047 ST-FR-MEDIA-006 — 资产搜索-系统业务验收

- **对应要件：** FR-MEDIA-006 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：指定索引规模下按分页检索，结果遵循权限隔离。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 048 ST-FR-MEDIA-007 — 媒体预览安全-系统业务验收

- **对应要件：** FR-MEDIA-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：构造异常媒体文件不能访问宿主机密钥或其他租户对象。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 049 ST-FR-MEDIA-008 — 预览内容与源关联-系统业务验收

- **对应要件：** FR-MEDIA-008 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：源变化触发可识别的重新生成；源内容保持不可变。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 050 ST-FR-MEDIA-009 — 媒体交付-系统业务验收

- **对应要件：** FR-MEDIA-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：断网后续传与视频Seek测试通过；缓存不泄漏租户数据。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 051 ST-FR-MEDIA-010 — 许可与溯源-系统业务验收

- **对应要件：** FR-MEDIA-010 | 阶段 E2 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 跨多媒体格式、恶意文件、20GiB视频Range seek与多租户查询进行完整业务操作；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原文始终不变，预览隔离和授权合格；逐项验收：受限制素材无法被标记为可公开发布。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 052 ST-FR-XCHG-001 — S3后端适配-系统业务验收

- **对应要件：** FR-XCHG-001 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：同一测试数据在至少AWS S3与另一个自建S3实现上完整往返。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 053 ST-FR-XCHG-002 — 非S3供应商适配-系统业务验收

- **对应要件：** FR-XCHG-002 | 阶段 E2 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：增补适配器无需更改S3客户端公开接口。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 054 ST-FR-XCHG-003 — 统一端点-系统业务验收

- **对应要件：** FR-XCHG-003 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：原有客户端仅改Endpoint/密钥即可访问受支持S3 API。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 055 ST-FR-XCHG-004 — 后端能力矩阵-系统业务验收

- **对应要件：** FR-XCHG-004 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：要求WORM而后端不满足时禁止部署，不允许虚假成功。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 056 ST-FR-XCHG-005 — 存储策略与放置-系统业务验收

- **对应要件：** FR-XCHG-005 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：Dry-run输出每条规则的匹配原因和成本估计。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 057 ST-FR-XCHG-006 — 迁移作业-系统业务验收

- **对应要件：** FR-XCHG-006 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 两个Provider对同一版本赋予不同physical VersionId，抽取旧版/delete marker/metadata/Tag/Lock后杀死Worker；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Checkpoint重放不漏Version；public逻辑VersionId保持，源/目标SHA及length一致；对照原要件验收：百万对象迁移中断后恢复，不漏文件；核验摘要与元数据。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 058 ST-FR-XCHG-007 — 迁移预演-系统业务验收

- **对应要件：** FR-XCHG-007 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：存在不兼容Object Lock时必须报阻断项。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 059 ST-FR-XCHG-008 — 无损切换-系统业务验收

- **对应要件：** FR-XCHG-008 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 完成Cutover后在新主写入V2并收到ACK，随后模拟目标故障请求立即回旧源；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 旧源未追平时必须阻断/只读；反向追平验证后才能CAS切回，V2仍可GET；对照原要件验收：生产演练迁移失败可回退到源端，用户已确认写入不丢失。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 060 ST-FR-XCHG-009 — 双端校验-系统业务验收

- **对应要件：** FR-XCHG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 人为损坏目标某个历史Version和Tag，执行完整Verification；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 分别输出HashMismatch和MetadataMismatch，任何差异阻断Cutover；对照原要件验收：对人为损坏和缺失分别准确告警。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 061 ST-FR-XCHG-010 — 连续复制-系统业务验收

- **对应要件：** FR-XCHG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：乱序和网络分区不产生静默覆盖；复制滞后指标可见。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 062 ST-FR-XCHG-011 — 冲突控制-系统业务验收

- **对应要件：** FR-XCHG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：并发写同键情况下不允许最后写入静默掩盖冲突。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 063 ST-FR-XCHG-012 — 读取故障转移-系统业务验收

- **对应要件：** FR-XCHG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：故障转移返回一致性状态；未同步数据不得误报存在。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 064 ST-FR-XCHG-013 — 后端出口开放-系统业务验收

- **对应要件：** FR-XCHG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：第三方工具可从导出的数据重建完整键空间及版本关系。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 065 ST-FR-XCHG-014 — 成本与计量-系统业务验收

- **对应要件：** FR-XCHG-014 | 阶段 E2 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：成本报表能与供应商账单进行差额核对。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 066 ST-FR-XCHG-015 — 底层供应商异常隔离-系统业务验收

- **对应要件：** FR-XCHG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) ≥100万对象及历史/Tag/Retention混合集、随机断线迁移、切换后新写入再发起回滚；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 原件100% SHA+length一致；已ACK写不丢；新写入后禁止盲回滚；逐项验收：一个后端故障不会拖垮其它桶的正常请求。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 067 ST-FR-NATIVE-001 — 多节点横向扩展-系统业务验收

- **对应要件：** FR-NATIVE-001 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：扩容演练不需要停掉全部S3服务。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 068 ST-FR-NATIVE-002 — 纠删码/数据保护-系统业务验收

- **对应要件：** FR-NATIVE-002 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 依次验证6/6、5/6、4/6 durable receipt；在PUT步骤P2/P3/P4断电/断网；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 仅6片达标且Raft Commit后可ACK；其他进入orphan/unknown结果并可补偿；对照原要件验收：配置容错范围内故障可读取完整数据，超出范围进入安全只读/错误状态。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 069 ST-FR-NATIVE-003 — 故障域意识-系统业务验收

- **对应要件：** FR-NATIVE-003 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：放置审计证明碎片满足故障域策略。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 070 ST-FR-NATIVE-004 — 节点自愈与巡检-系统业务验收

- **对应要件：** FR-NATIVE-004 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：混合故障注入后数据摘要不变，进度和剩余风险可查询。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 071 ST-FR-NATIVE-005 — 存储一致性语义-系统业务验收

- **对应要件：** FR-NATIVE-005 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 对同Key并发覆盖/删/GET/HEAD/LIST并在leader切换和快照压缩时执行；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK后读一致、无半对象；单Bucket单Group，跨range模式未证明前disabled；对照原要件验收：ACK后GET/HEAD/LIST（在规定作用域）立即可见，不出现半对象。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** REV-010:跨range强LIST未证明前只测单Bucket单Group

## 072 ST-FR-NATIVE-006 — 数据与控制面故障隔离-系统业务验收

- **对应要件：** FR-NATIVE-006 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：单点注入与恢复试验通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 073 ST-FR-NATIVE-007 — 容量水位保护-系统业务验收

- **对应要件：** FR-NATIVE-007 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：容量接近上限能预测并安全拒绝写入。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 074 ST-FR-NATIVE-008 — 数据损坏检测-系统业务验收

- **对应要件：** FR-NATIVE-008 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：注入存储位错误后检测到并在冗余足够时自愈。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 075 ST-FR-NATIVE-009 — 故障演练接口-系统业务验收

- **对应要件：** FR-NATIVE-009 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 3AZ / 6节点K4M2环境按故障注入矩阵进行AZ丢失、片腐败、Quorum切换及恢复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 任何ACK对象在声明容错内可完整恢复；失AZ默认只读或安全拒绝写；逐项验收：每个发布候选版本执行故障注入测试。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 076 ST-FR-SEC-001 — SSO与身份联邦-系统业务验收

- **对应要件：** FR-SEC-001 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：SSO登录/登出/禁用账号后会话生效性经测试。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 077 ST-FR-SEC-002 — IAM/RBAC/ABAC-系统业务验收

- **对应要件：** FR-SEC-002 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：尝试越权访问、横向提权和策略覆盖均失败。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 078 ST-FR-SEC-003 — 短期凭据-系统业务验收

- **对应要件：** FR-SEC-003 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：过期Token和已撤销AccessKey被拒绝并审计。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 079 ST-FR-SEC-004 — 传输加密-系统业务验收

- **对应要件：** FR-SEC-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：未加密入口默认关闭；TLS配置可扫描验证。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 080 ST-FR-SEC-005 — 静态加密与密钥管理-系统业务验收

- **对应要件：** FR-SEC-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 在KMS revoke/轮换/timeout期间上传读历史Version，变更DEK wrapper；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不明文回退，旧版授权读仍可验证或明确拒绝；审计完整；对照原要件验收：KMS故障不会造成明文回退，轮换后的旧数据可访问。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 081 ST-FR-SEC-006 — 审计追踪-系统业务验收

- **对应要件：** FR-SEC-006 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：追踪一个Git Commit至对应OID和S3 Version与访问日志。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 082 ST-FR-SEC-007 — 不可篡改审计-系统业务验收

- **对应要件：** FR-SEC-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 阻断外部审计Sink 30min、重启outbox sender、篡改Sink中历史记录；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 日志队列不丢且事件去重；篡改hash链告警；普通Ops无法重写；对照原要件验收：系统管理员不能通过普通运维权限覆盖已封存审计记录。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 083 ST-FR-SEC-008 — 恶意软件与内容控制-系统业务验收

- **对应要件：** FR-SEC-008 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：恶意媒体可隔离并可追查，原始SHA-256和隔离动作有证据。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 084 ST-FR-SEC-009 — 管理面防护-系统业务验收

- **对应要件：** FR-SEC-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：删除存储池、强制解锁、迁移切换触发审批规则。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 085 ST-FR-SEC-010 — 安全加固与供应链-系统业务验收

- **对应要件：** FR-SEC-010 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：关键漏洞阻止正式发布，SBOM可下载。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 086 ST-FR-SEC-011 — 合法持有与数据隔离-系统业务验收

- **对应要件：** FR-SEC-011 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：租户删除前检测WORM和Git活跃引用，依法合规执行。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 087 ST-FR-SEC-012 — 秘密管理-系统业务验收

- **对应要件：** FR-SEC-012 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：数据导出和日志不包含明文AccessKey/SecretKey。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 088 ST-FR-SEC-013 — 勒索防护-系统业务验收

- **对应要件：** FR-SEC-013 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：批量误删后可以按保留策略恢复；模拟攻击产生报警。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 089 ST-FR-SEC-014 — 合规声明边界-系统业务验收

- **对应要件：** FR-SEC-014 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 角色越权矩阵、KMS中断、Retention绕过、恶意上传和勒索模拟；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 敏感资源默认拒绝，审计/加密/保护证明齐全；逐项验收：发布材料包含适用范围、例外、证据和未认证声明。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 090 ST-FR-OPS-001 — Web管理控制台-系统业务验收

- **对应要件：** FR-OPS-001 | 阶段 E0 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：任务列表和对象详情三步内可进入，角色不可见不属于自己的资源。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 091 ST-FR-OPS-002 — 版本关系可视化-系统业务验收

- **对应要件：** FR-OPS-002 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：定位一项历史资源无需理解后端私有路径。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 092 ST-FR-OPS-003 — 迁移与复制工作台-系统业务验收

- **对应要件：** FR-OPS-003 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：操作可审计；危险动作有影响预览。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 093 ST-FR-OPS-004 — 媒体浏览器-系统业务验收

- **对应要件：** FR-OPS-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：图像、视频、3D预览异常不阻断下载。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 094 ST-FR-OPS-005 — 多语言-系统业务验收

- **对应要件：** FR-OPS-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：语言切换不改变权限/对象数据；日语信息自然一致。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 095 ST-FR-OPS-006 — 无障碍与认知负荷-系统业务验收

- **对应要件：** FR-OPS-006 | 阶段 E2 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：键盘完成桶创建、恢复版本和权限查询，核心流程可用性评审通过。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 096 ST-FR-OPS-007 — CLI与配置即代码-系统业务验收

- **对应要件：** FR-OPS-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：操作与Web实现相同权限与审计策略。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 097 ST-FR-OPS-008 — API文档与SDK示例-系统业务验收

- **对应要件：** FR-OPS-008 | 阶段 E2 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：新开发者按文档完成上传、Git LFS和迁移样例。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 098 ST-FR-OPS-009 — 审计查询与报告-系统业务验收

- **对应要件：** FR-OPS-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：报告可重现追查一次管理员动作与Git修改。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 099 ST-FR-OPS-010 — 监控与告警-系统业务验收

- **对应要件：** FR-OPS-010 | 阶段 E0 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：Grafana/兼容仪表板可读取关键指标并按阈值触发。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 100 ST-FR-OPS-011 — 零停机滚动升级-系统业务验收

- **对应要件：** FR-OPS-011 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) N/N-1混部滚动更替Meta voters、数据节点和Gateway，故障在Meta schema迁移中发生；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** quorum不丢、旧版能读新版本记录或进入阻断回退，备份可恢复；对照原要件验收：在允许的单节点轮替中读写持续；失败能按预案恢复。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 101 ST-FR-OPS-012 — 备份与恢复-系统业务验收

- **对应要件：** FR-OPS-012 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：仅从备份与对象数据可复原Git映射和用户可读对象。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 102 ST-FR-OPS-013 — 异常与根因信息-系统业务验收

- **对应要件：** FR-OPS-013 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：S3错误+后台trace可串联到具体后端与操作。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 103 ST-FR-OPS-014 — 运维 runbook-系统业务验收

- **对应要件：** FR-OPS-014 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：演练人员不依赖开发者介入完成预定操作。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 104 ST-FR-OPS-015 — 服务水平报告-系统业务验收

- **对应要件：** FR-OPS-015 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 真实Console/CLI部署、升级、DR操作、告警与轮值runbook演练；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 管理员可完成流程且不绕过审批；日志可复现事件与时间；逐项验收：月报按原始监控数据复算一致。
- **证据：** 完整测试日志+故障时间线+监控snapshot+原始SHA/Version清单+验收签署
- **方法：** E2E/chaos/manual signoff
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 105 ST-NFR-AVA-001 — 生产可用率-指标与质量门禁

- **对应要件：** NFR-AVA-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『E1至少3故障域、正常维护口径按SLA另行约定』下测得『月度 S3 API 可用率≥99.95%（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 106 ST-NFR-AVA-002 — 确认写入持久性-指标与质量门禁

- **对应要件：** NFR-AVA-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任意单节点/单磁盘故障及配置允许的故障数量范围内』下测得『已返回2xx的对象不得丢失，损坏可自动检出』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 107 ST-NFR-AVA-003 — 站点恢复-指标与质量门禁

- **对应要件：** NFR-AVA-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『异地主备配置已启用、依赖服务可用』下测得『生产RTO≤60分钟；异步跨站RPO≤15分钟（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 108 ST-NFR-AVA-004 — 同站数据一致性-指标与质量门禁

- **对应要件：** NFR-AVA-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『针对primary、ACK后单键与LIST可见性』下测得『ACK后GET/HEAD/LIST强一致；跨站异步复制单独声明』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 109 ST-NFR-AVA-005 — WORM恢复安全-指标与质量门禁

- **对应要件：** NFR-AVA-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『磁盘失效、节点回滚、时钟漂移』下测得『保留版本在保留期限内不得通过应用API提前删除』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 110 ST-NFR-AVA-006 — 恢复演练周期-指标与质量门禁

- **对应要件：** NFR-AVA-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产变更与季度计划』下测得『季度DR演练；每月备份恢复抽样验证』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 111 ST-NFR-PER-001 — S3对象延迟-指标与质量门禁

- **对应要件：** NFR-PER-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量
- **测试步骤：** 1) B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量；2) 按目标负载预热并持续计量p95/p99/吞吐/RSS与公平性；记录95%置信区间和异常波形；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『B1基线：6节点×16vCPU×64GiB×NVMe×25GbE，3故障域，1MiB对象，300并发』下测得『GET p95≤150ms；PUT p95≤300ms（不含公网）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 112 ST-NFR-PER-002 — 数据吞吐-指标与质量门禁

- **对应要件：** NFR-PER-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量
- **测试步骤：** 1) B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量；2) 按目标负载预热并持续计量p95/p99/吞吐/RSS与公平性；记录95%置信区间和异常波形；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『B1基线，100MiB顺序对象流、预热后连续运行30分钟』下测得『集群聚合读取吞吐≥1GiB/s，具体由硬件验收确认』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 113 ST-NFR-PER-003 — 存储规模-指标与质量门禁

- **对应要件：** NFR-PER-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量
- **测试步骤：** 1) B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量；2) 按目标负载预热并持续计量p95/p99/吞吐/RSS与公平性；记录95%置信区间和异常波形；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『E1设计容量目标，部署规模/账本成本另行合意』下测得『≥1亿对象、≥1PiB逻辑命名空间可寻址（压力验证可分层）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 114 ST-NFR-PER-004 — 弹性扩容-指标与质量门禁

- **对应要件：** NFR-PER-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量
- **测试步骤：** 1) B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量；2) 按目标负载预热并持续计量p95/p99/吞吐/RSS与公平性；记录95%置信区间和异常波形；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『B1基线扩容1节点，无计划全站停机』下测得『扩容期间原API保持可用，数据完整，性能下降范围形成报告』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 115 ST-NFR-PER-005 — Git媒体规模-指标与质量门禁

- **对应要件：** NFR-PER-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量
- **测试步骤：** 1) B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量；2) 按目标负载预热并持续计量p95/p99/吞吐/RSS与公平性；记录95%置信区间和异常波形；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『单仓库10万LFS对象、单对象≥20GiB』下测得『可上传/下载/恢复/索引且不出现O(size)内存增长』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 116 ST-NFR-PER-006 — 并发公平性-指标与质量门禁

- **对应要件：** NFR-PER-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量
- **测试步骤：** 1) B1=6节点16vCPU/64GiB/NVMe/25GbE/3故障域；固定1MiB/100MiB负载与300并发，记录CPU/网络/对象总量；2) 按目标负载预热并持续计量p95/p99/吞吐/RSS与公平性；记录95%置信区间和异常波形；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『3租户同时进行S3大对象上传、下载与迁移』下测得『限流生效，任一租户无法持续耗尽全局带宽/IOPS』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 117 ST-NFR-OPS-001 — 指标覆盖-指标与质量门禁

- **对应要件：** NFR-OPS-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据
- **测试步骤：** 1) 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据；2) 重复运行部署/升级/回滚/恢复与告警场景，抽样核对审计outbox和操作结果；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产运行中的API、节点、KMS、复制、Git网关』下测得『请求、错误、延迟、容量、校验、排队、滞后均有指标』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 118 ST-NFR-OPS-002 — 部署和回滚-指标与质量门禁

- **对应要件：** NFR-OPS-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据
- **测试步骤：** 1) 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据；2) 重复运行部署/升级/回滚/恢复与告警场景，抽样核对审计outbox和操作结果；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『指定的K8s/VM环境』下测得『声明式安装、滚动升级、版本回退操作步骤经演练通过』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 119 ST-NFR-OPS-003 — 数据修复透明-指标与质量门禁

- **对应要件：** NFR-OPS-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据
- **测试步骤：** 1) 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据；2) 重复运行部署/升级/回滚/恢复与告警场景，抽样核对审计outbox和操作结果；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任何自愈或恢复操作』下测得『有操作日志、校验报告、版本证据，不允许静默覆盖』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 120 ST-NFR-OPS-004 — 代码质量-指标与质量门禁

- **对应要件：** NFR-OPS-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据
- **测试步骤：** 1) 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据；2) 重复运行部署/升级/回滚/恢复与告警场景，抽样核对审计outbox和操作结果；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『核心S3、Git LFS、元数据、授权、迁移代码』下测得『自动化测试覆盖率目标≥90%；关键路径100%分支用例设计』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 121 ST-NFR-OPS-005 — 版本升级兼容-指标与质量门禁

- **对应要件：** NFR-OPS-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据
- **测试步骤：** 1) 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据；2) 重复运行部署/升级/回滚/恢复与告警场景，抽样核对审计outbox和操作结果；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『Rolling upgrade N/N-1服务混部』下测得『API、数据格式与元数据迁移可回退或前置备份校验』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 122 ST-NFR-OPS-006 — 故障响应-指标与质量门禁

- **对应要件：** NFR-OPS-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据
- **测试步骤：** 1) 受控K8s/VM部署、值班告警/Runbook、Git+KMS+审计、真实备份数据；2) 重复运行部署/升级/回滚/恢复与告警场景，抽样核对审计outbox和操作结果；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『Sev1/Sev2事件定义合意后』下测得『Sev1自动告警≤5min，操作手册有明确责任人和升级路径』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 123 ST-NFR-MIG-001 — 迁移数据准确度-指标与质量门禁

- **对应要件：** NFR-MIG-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『≥100万对象，含非当前版本、标签、锁定对象』下测得『对象SHA-256及字节长度 100%一致；元数据差异全部列明』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 124 ST-NFR-MIG-002 — 迁移中断恢复-指标与质量门禁

- **对应要件：** NFR-MIG-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『随机kill迁移进程和目标后端5xx』下测得『重启后接续、不出现漏项；无法迁移对象附异常报告』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 125 ST-NFR-MIG-003 — 迁移回滚-指标与质量门禁

- **对应要件：** NFR-MIG-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『源站保留至独立验证和审批完成』下测得『失败切换可逆且不会覆盖源站已确认写入』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 126 ST-NFR-MIG-004 — 外部兼容性-指标与质量门禁

- **对应要件：** NFR-MIG-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『AWS CLI、aws-sdk-js v3、boto3、Go/Rust S3 SDK、git-lfs』下测得『宣称兼容功能通过协议回归测试≥99%；0个阻断级差异』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 127 ST-NFR-SEC-001 — 权限拒绝-指标与质量门禁

- **对应要件：** NFR-SEC-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任意匿名、过期、跨租户、撤销凭据』下测得『敏感资源默认拒绝；跨租户访问成功数=0』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 128 ST-NFR-SEC-002 — 加密-指标与质量门禁

- **对应要件：** NFR-SEC-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产外部/东西向连接、静态数据』下测得『TLS1.2+；静态强加密并依赖受控KMS；禁用明文后退』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 129 ST-NFR-SEC-003 — 凭据处理-指标与质量门禁

- **对应要件：** NFR-SEC-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『CI/CD、日志、控制台、事故现场』下测得『不能输出密钥明文；密钥轮换和吊销符合审计要求』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 130 ST-NFR-SEC-004 — 漏洞要求-指标与质量门禁

- **对应要件：** NFR-SEC-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『正式版本发布前及修复SLAs』下测得『0个已知未接受的Critical漏洞；高危经审批例外』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 131 ST-NFR-SEC-005 — 审计保留-指标与质量门禁

- **对应要件：** NFR-SEC-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『关键安全日志含Object Lock/KMS/Git操作』下测得『审计事件可关联且在外部不可变存储受控保留≥365天（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 132 ST-NFR-SEC-006 — 供应链验证-指标与质量门禁

- **对应要件：** NFR-SEC-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『正式发行包、容器及三方依赖』下测得『SBOM齐全、构建物签名、授权许可证扫描完成』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 133 ST-NFR-ENV-001 — 支持环境-指标与质量门禁

- **对应要件：** NFR-ENV-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『Linux amd64/arm64、容器、K8s；Windows为CLI/客户端』下测得『支持矩阵中的环境可重复部署且升级回滚通过』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 134 ST-NFR-ENV-002 — 资源效率-指标与质量门禁

- **对应要件：** NFR-ENV-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『控制面中等负载（B1），监控处理器与内存』下测得『资源预算以性能基线测试建立，不以未验证的绝对值承诺』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 135 ST-NFR-ENV-003 — 离线可维护性-指标与质量门禁

- **对应要件：** NFR-ENV-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『企业内网环境』下测得『无公网时安装镜像、文档、许可证/密钥方案可正常运作』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 136 ST-NFR-ENV-004 — 地域节能策略-指标与质量门禁

- **对应要件：** NFR-ENV-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『冷热层存储/生命周期需要遵守WORM及数据驻留』下测得『可呈现资源耗用及预估迁移成本，不因优化误删受保护版本』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** Performance/chaos/DR/manual approval
- **执行结果：** Not Run
- **阻断说明：** CR-001~008按适用项待签署；数值基线需事先合意

## 137 ST-UC-01 — 开发者：在GitGit仓库提交4GB视频

- **对应要件：** FR-GG-005, FR-MV-007, FR-MV-019 | 阶段 E1 | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以开发者身份在GitGit原生客户端开始测试；2) 执行在GitGit仓库提交4GB视频；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** GitGit原生上传Manifest、提交签名Receipt并完成Ref CAS；下载精确还原，LFS为兼容用例；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 138 ST-UC-02 — 美术：比较GitGit中两个Commit的媒体变化

- **对应要件：** FR-GG-008, FR-MV-013 | 阶段 E0 | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以美术身份在GitGit原生客户端开始测试；2) 执行比较GitGit中两个Commit的媒体变化；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 展示原媒体、版本源和可用对比，失败不修改原文件。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 139 ST-UC-03 — 管理员：从MinIO S3迁移到AWS S3

- **对应要件：** FR-XCHG-006, FR-XCHG-008, FR-XCHG-009 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以管理员身份在GitGit原生客户端开始测试；2) 执行从MinIO S3迁移到AWS S3；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** Dry-run差异、传输、校验、切换、回滚；原客户端端点保持稳定。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 140 ST-UC-04 — 运维：单节点损坏和数据自愈

- **对应要件：** FR-NATIVE-002, FR-NATIVE-004 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以运维身份在GitGit原生客户端开始测试；2) 执行单节点损坏和数据自愈；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 可用性满足故障域承诺，自愈后摘要一致。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 141 ST-UC-05 — 合规员：对审计资料设置法律保留

- **对应要件：** FR-SEC-007 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以合规员身份在GitGit原生客户端开始测试；2) 执行对审计资料设置法律保留；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 锁定版本不能被普通管理员删除，审计证明完整。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 142 ST-UC-06 — 审计人员：追踪GitGit提交的全部媒体内容

- **对应要件：** FR-GG-002, FR-MV-027 | 阶段 E0 | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以审计人员身份在GitGit原生客户端开始测试；2) 执行追踪GitGit提交的全部媒体内容；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 返回ref、commit、OID、S3版本、授权与操作历史。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 143 ST-UC-07 — 开发者：用Boto3通过统一S3端点获取媒体

- **对应要件：** FR-XCHG-003 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以开发者身份在GitGit原生客户端开始测试；2) 执行用Boto3通过统一S3端点获取媒体；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 现有S3 SDK不需要专用客户端。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 144 ST-UC-08 — 管理员：后端不具WORM能力但项目强制WORM

- **对应要件：** FR-XCHG-004 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以管理员身份在GitGit原生客户端开始测试；2) 执行后端不具WORM能力但项目强制WORM；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 路由/迁移操作被阻断，记录明确原因。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 145 ST-UC-09 — 维护者：删除分支后清理孤儿媒体

- **对应要件：** FR-GG-016, FR-MV-028 | 阶段 E1 | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以维护者身份在GitGit原生客户端开始测试；2) 执行删除分支后清理孤儿媒体；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 仅回收不可达、已过宽限期且不受保留策略保护的媒体。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 146 ST-UC-10 — 制作团队：多用户使用GitGit锁协作编辑3D模型

- **对应要件：** FR-GG-009, FR-MV-016 | 阶段 E1 | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以制作团队身份在GitGit原生客户端开始测试；2) 执行多用户使用GitGit锁协作编辑3D模型；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 通过LFS锁阻止被保护路径并发覆盖。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 147 ST-UC-11 — 运维：大规模后端限流和重试

- **对应要件：** FR-XCHG-015, NFR-PER-006 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以运维身份在GitGit原生客户端开始测试；2) 执行大规模后端限流和重试；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 有背压与指数退避，对其他租户影响受限。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 148 ST-UC-12 — 运维：从备份重建元数据控制面

- **对应要件：** FR-OPS-012, NFR-AVA-003 | 阶段 E0/E1/E2依所引用FR | 优先级 MUST
- **前提：** GitGit原生仓库、MinIOS3真实内容服务、两租户权限样本、受控S3后端及业务可复核媒体Hash
- **测试步骤：** 1) 以运维身份在GitGit原生客户端开始测试；2) 执行从备份重建元数据控制面；3) 验证出错、未授权、断网重试和相关仓库/桶权限；4) 核对GitGit Commit/Manifest/Chunk SHA及审计；5) 与业务使用者签署结果。
- **判定：** 恢复Git到OID映射、权限及桶配置，进行内容核验。；所有相关媒体原件可逐字节恢复；LFS仅测试兼容适配不替代GitGit原生功能。
- **证据：** 业务回放视频/脚本+Git log+sha校验+S3 Version列表+签名验收
- **方法：** E2E business acceptance
- **执行结果：** Not Run
- **阻断说明：** 缺少真实GitGit Core/测试环境即Blocked，当前Not Run

## 149 ST-ADV-B1-READWRITE — 区分读SLO/写SLO

- **对应要件：** FR-OPS-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 6节点4+2失AZ后已经存在对象可恢复，不能继续ACK不合规新写；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 6节点4+2失AZ后已经存在对象可恢复，不能继续ACK不合规新写；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 150 ST-ADV-GIT-REAL — GitGit Rust Core真实PreReceive Gate

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 启动真实GitGit Rust Core及MinIOS3；2) 创建新Manifest及DurableReceipt；3) 执行Commit Prepare/Ref CAS并注入错误/乱序/掉电；4) 核对Ref/AssetRevision/Manifest SHA/安全GC与审计Outbox；5) 若涉及LFS，请作为独立可选适配测试。
- **判定：** 不接受客户端自己声明的OID，原子Ref CAS前拦截；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 151 ST-ADV-GIT-COMMITS — 100k AssetRevision / Manifest与20GiB文件

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 启动真实GitGit Rust Core及MinIOS3；2) 创建新Manifest及DurableReceipt；3) 执行Commit Prepare/Ref CAS并注入错误/乱序/掉电；4) 核对Ref/AssetRevision/Manifest SHA/安全GC与审计Outbox；5) 若涉及LFS，请作为独立可选适配测试。
- **判定：** 单仓库LFS push/clone/index，RSS不随对象大小线性增长；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 152 ST-ADV-MIG-MILLION — 百万版本校验

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 100万对象，包含旧Version/marker/WORM，SHA+长度100%一致；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 100万对象，包含旧Version/marker/WORM，SHA+长度100%一致；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 153 ST-ADV-MIG-KILL — kill迁移100次恢复

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) checkpoint重放不丢Version、不重复改变主写；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** checkpoint重放不丢Version、不重复改变主写；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 154 ST-ADV-MIG-CUTOVER — Cutover后新写入回滚

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 只有reverse catchup成功才切回，任何ACK对象不丢；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 只有reverse catchup成功才切回，任何ACK对象不丢；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 155 ST-ADV-MIG-OLDVER — VersionId跨供应商差异

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 原public URL+VersionId切换前后读取同一摘要；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 原public URL+VersionId切换前后读取同一摘要；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 156 ST-ADV-MIG-RESIDENCY — 数据驻留违规阻断

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 禁止跨区域Job且有审计；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 禁止跨区域Job且有审计；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 157 ST-ADV-S3-LIST-MANY — 千万级分页与并发写

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 单Bucket快照分页无重复/漏Key，token过期按合同拒绝；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 单Bucket快照分页无重复/漏Key，token过期按合同拒绝；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 158 ST-ADV-S3-VERSIONS — Suspended/Null/Marker矩阵

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 所有合法历史版本可读、锁保护不退化；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 所有合法历史版本可读、锁保护不退化；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 159 ST-ADV-S3-PRESIGNED — 短链攻击模拟

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 更换method/key/tenant/signature/expiry均拒绝；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 更换method/key/tenant/signature/expiry均拒绝；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 160 ST-ADV-S3-20G — 20GiB读写Range和断线

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 峰值RSS受控，边界内容SHA相同；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 峰值RSS受控，边界内容SHA相同；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 161 ST-ADV-NATIVE-ZONE — 失AZ后只读保护

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 4/6可读，默认拒绝新写直到布局恢复；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 4/6可读，默认拒绝新写直到布局恢复；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 162 ST-ADV-NATIVE-SPLIT — Raft网络分区

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 仅多数派可接收目录提交，旧term失败；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 仅多数派可接收目录提交，旧term失败；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 163 ST-ADV-NATIVE-BITROT — 任意单片位翻转

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 发现错片并重建，原文Sha256恒等；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 发现错片并重建，原文Sha256恒等；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 164 ST-ADV-NATIVE-REBAL — 扩容与负载影响

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 前台API持续（按能力），不损坏/错位；提供性能退化数据；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 前台API持续（按能力），不损坏/错位；提供性能退化数据；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 165 ST-ADV-DR-RESTORE — 四联恢复演练

- **对应要件：** FR-OPS-012 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) Meta+shards+Git refs+KMS恢复，RTO/RPO有测量；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** Meta+shards+Git refs+KMS恢复，RTO/RPO有测量；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 166 ST-ADV-DR-RPO — 跨站异步链路中断

- **对应要件：** FR-OPS-012 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 显示真实lag且不把缺失版本当存在；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 显示真实lag且不把缺失版本当存在；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 167 ST-ADV-AUDIT-WORM — 操作员删除审计

- **对应要件：** FR-SEC-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 独立不可变sink拒绝，hash链异常被告警；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 独立不可变sink拒绝，hash链异常被告警；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 168 ST-ADV-SEC-RANSOM — 模拟勒索批量删

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 历史受保护版本可按保留恢复，安全告警触发；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 历史受保护版本可按保留恢复，安全告警触发；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 169 ST-ADV-SEC-OID — 跨租户侧信道

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 猜OID下载/计数/时间差不得泄漏被测Object；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 猜OID下载/计数/时间差不得泄漏被测Object；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 170 ST-ADV-MEDIA-SAFE — 恶意媒体和插件沙盒

- **对应要件：** FR-OPS-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 不能访问宿主/其它租户/KMS；原件保留隔离证据；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 不能访问宿主/其它租户/KMS；原件保留隔离证据；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 171 ST-ADV-OPS-ROLL — N/N-1混部滚动更新

- **对应要件：** FR-OPS-012 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 逐meta voter升级保持quorum，rollback不丢确认版本；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 逐meta voter升级保持quorum，rollback不丢确认版本；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 172 ST-ADV-OPS-OFFLINE — 内网离线安装测试

- **对应要件：** FR-OPS-012 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 本地后端可用，AWS外网不可达时诚实报告；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 本地后端可用，AWS外网不可达时诚实报告；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 173 ST-ADV-SUPPLYCHAIN — Rust发行许可与漏洞门禁

- **对应要件：** FR-OPS-001 | 阶段 E0/E1按场景 | 优先级 P0
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) SBOM签名与CVE/license审核产物齐全；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** SBOM签名与CVE/license审核产物齐全；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 174 ST-FR-GG-001 — GitGit仓库权威 — ST

- **对应要件：** FR-GG-001 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“GitGit仓库权威”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入重复并发CAS、期待旧Ref修订号，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；停用所有外部Git兼容接口，仍能完成创建仓库、提交、分支与历史恢复。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 175 ST-FR-GG-002 — 不可变提交图 — ST

- **对应要件：** FR-GG-002 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“不可变提交图”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入主节点断电与ACK丢失，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；同输入稳定得到同Commit ID；不能通过正常API改写历史对象。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 176 ST-FR-GG-003 — 原子Ref CAS — ST

- **对应要件：** FR-GG-003 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“原子Ref CAS”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入重名路径和Unicode规范化，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；两名并发写者仅一个成功；失败者看见明确冲突且不产生部分Ref。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 177 ST-FR-GG-004 — 仓库快照 — ST

- **对应要件：** FR-GG-004 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“仓库快照”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Outbox重复/乱序/永久缺失，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；重命名/删除之后checkout旧Commit按字节还原全部文件。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 178 ST-FR-GG-005 — 两阶段提交门禁 — ST

- **对应要件：** FR-GG-005 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“两阶段提交门禁”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入无权限读、票据过期与重试，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；缺片或Receipt过期时拒绝Ref发布，且保留可补偿事务。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 179 ST-FR-GG-006 — 幂等确认与Outbox — ST

- **对应要件：** FR-GG-006 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“幂等确认与Outbox”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入断网恢复时Ref已改变，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；断网+重放+乱序事件后两侧谱系最终对账一致，不产生重复版本。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 180 ST-FR-GG-007 — 树级变更与重命名 — ST

- **对应要件：** FR-GG-007 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“树级变更与重命名”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Tag保护、ForcePush、旧Snapshot，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；重命名保留AssetId及全部历史；路径冲突不得静默覆盖。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 181 ST-FR-GG-008 — 分支差异查询 — ST

- **对应要件：** FR-GG-008 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“分支差异查询”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入仓库级ACL撤销与GC竞态，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；带1000个资产的样本差异结果可按AssetId/RevisionId复算。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 182 ST-FR-GG-009 — 原生资产锁 — ST

- **对应要件：** FR-GG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“原生资产锁”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入重复并发CAS、期待旧Ref修订号，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；并发锁冲突拒绝覆盖，强制解锁有审计并更新fencing token。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 183 ST-FR-GG-010 — 离线工作区日志 — ST

- **对应要件：** FR-GG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“离线工作区日志”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入主节点断电与ACK丢失，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；断网修改恢复后不误覆盖远端并提示冲突。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 184 ST-FR-GG-011 — 稀疏工作区与懒加载 — ST

- **对应要件：** FR-GG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“稀疏工作区与懒加载”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入重名路径和Unicode规范化，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；未选中的20GiB媒体不下载，选中内容下载SHA与Manifest一致。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 185 ST-FR-GG-012 — 三方合并引擎 — ST

- **对应要件：** FR-GG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“三方合并引擎”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Outbox重复/乱序/永久缺失，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；不可安全合并时返回显式冲突，绝不自动选取后写者。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 186 ST-FR-GG-013 — 冲突决议审计 — ST

- **对应要件：** FR-GG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“冲突决议审计”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入无权限读、票据过期与重试，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；合并结果可复查全部parent和resolve原因。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 187 ST-FR-GG-014 — 媒体评审流 — ST

- **对应要件：** FR-GG-014 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“媒体评审流”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入断网恢复时Ref已改变，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；未获必需审批无法发布指定受保护分支。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 188 ST-FR-GG-015 — 依赖变更影响 — ST

- **对应要件：** FR-GG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“依赖变更影响”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Tag保护、ForcePush、旧Snapshot，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；修改纹理后定位全部引用该纹理的模型且不跨分支误报。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 189 ST-FR-GG-016 — 历史可达性水位 — ST

- **对应要件：** FR-GG-016 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“历史可达性水位”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入仓库级ACL撤销与GC竞态，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；丢事件或正在并发恢复时GC fail-closed且不得误删仍可达块。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 190 ST-FR-GG-017 — 组织级授权 — ST

- **对应要件：** FR-GG-017 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“组织级授权”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入重复并发CAS、期待旧Ref修订号，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；撤权与过期读票据不会绕开资源级ACL。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 191 ST-FR-GG-018 — 受保护提交策略 — ST

- **对应要件：** FR-GG-018 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“受保护提交策略”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入主节点断电与ACK丢失，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；无权force-update拒绝且所有策略决策留痕。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 192 ST-FR-GG-019 — 标准Git迁入隔离 — ST

- **对应要件：** FR-GG-019 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 由业务角色从客户端执行“标准Git迁入隔离”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入重名路径和Unicode规范化，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；禁用互操作插件后原生核心可运行；迁移映射差异清晰可查。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 193 ST-FR-GG-020 — 提交与资产恢复 — ST

- **对应要件：** FR-GG-020 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 由业务角色从客户端执行“提交与资产恢复”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Outbox重复/乱序/永久缺失，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；模拟跨存储迁移后历史恢复字节正确。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 194 ST-FR-MV-001 — 稳定资产标识 — ST

- **对应要件：** FR-MV-001 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“稳定资产标识”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Chunk大小边界与文件前段插入，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；移动/改名不改变AssetId，更新内容必生成新Revision。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 195 ST-FR-MV-002 — 媒体版本记录 — ST

- **对应要件：** FR-MV-002 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“媒体版本记录”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入篡改Chunk/Manifest与重试校验，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；同一Asset存在分叉与合并时可画出完整版本DAG。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 196 ST-FR-MV-003 — 内容清单 — ST

- **对应要件：** FR-MV-003 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“内容清单”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入跨租户去重存在性推断，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；任意读取块可按清单重组并与原始SHA-256完全一致。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 197 ST-FR-MV-004 — 内容定义分块 — ST

- **对应要件：** FR-MV-004 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“内容定义分块”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入无法解析媒体并触发隔离限额，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；前部插入小内容后后续大部分相同Chunk能够复用，参数与分块版本固定。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 198 ST-FR-MV-005 — 强摘要验证 — ST

- **对应要件：** FR-MV-005 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“强摘要验证”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Adapter升级、能力降级与许可证失败，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；随机篡改任意Chunk或Manifest必被拒绝/隔离，不能返回成功ACK。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 199 ST-FR-MV-006 — 安全租户去重 — ST

- **对应要件：** FR-MV-006 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“安全租户去重”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入三方merge冲突与并行修改，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；Tenant B测时/响应无法判定Tenant A内容是否存在，B无权取A的Chunk。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 200 ST-FR-MV-007 — 断点续传 — ST

- **对应要件：** FR-MV-007 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“断点续传”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入S3物理版本变化及迁移Cutover，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；20GiB模拟流断线后仅补传缺块，不重复生成同一可见Revision。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 201 ST-FR-MV-008 — 增量下载 — ST

- **对应要件：** FR-MV-008 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“增量下载”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入GC/回滚和已ACK资产的并发可达性，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；部分请求字节/offset正确，内存占用不随文件总大小线性增长。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 202 ST-FR-MV-009 — 原件不可变 — ST

- **对应要件：** FR-MV-009 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“原件不可变”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Chunk大小边界与文件前段插入，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；所有派生对象sha不同空间，重取旧Commit的原件字节完全一致。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 203 ST-FR-MV-010 — 媒体探测 — ST

- **对应要件：** FR-MV-010 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“媒体探测”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入篡改Chunk/Manifest与重试校验，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；伪造扩展名不能自动触发高风险解析器，未知类型安全保存。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 204 ST-FR-MV-011 — 可扩展元数据 — ST

- **对应要件：** FR-MV-011 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“可扩展元数据”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入跨租户去重存在性推断，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；未知字段可往返且Schema升级不破坏原始数据。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 205 ST-FR-MV-012 — 媒体适配器统一接口 — ST

- **对应要件：** FR-MV-012 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“媒体适配器统一接口”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入无法解析媒体并触发隔离限额，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；不支持能力返回Unsupported而非默认成功，Adapter升级不修改旧Revision。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 206 ST-FR-MV-013 — 图像版本差异 — ST

- **对应要件：** FR-MV-013 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“图像版本差异”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Adapter升级、能力降级与许可证失败，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；参考图golden数据可复算热区，PSDs不支持图层时降级为渲染图比较。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 207 ST-FR-MV-014 — 视频时间线差异 — ST

- **对应要件：** FR-MV-014 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“视频时间线差异”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入三方merge冲突与并行修改，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；变更片段时间码与样本golden一致，不能把非确定性转码当原内容证据。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 208 ST-FR-MV-015 — 音频版本差异 — ST

- **对应要件：** FR-MV-015 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“音频版本差异”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入S3物理版本变化及迁移Cutover，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；参考音频能定位差异区间，未标准化采样率须显式提示。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 209 ST-FR-MV-016 — 三维资产差异 — ST

- **对应要件：** FR-MV-016 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“三维资产差异”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入GC/回滚和已ACK资产的并发可达性，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；相同拓扑不同材质及相同外观不同网格被正确区别。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 210 ST-FR-MV-017 — 安全合并能力声明 — ST

- **对应要件：** FR-MV-017 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“安全合并能力声明”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Chunk大小边界与文件前段插入，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；两个不可合并二进制冲突绝不自动丢失一方修改。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 211 ST-FR-MV-018 — 资源依赖DAG — ST

- **对应要件：** FR-MV-018 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“资源依赖DAG”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入篡改Chunk/Manifest与重试校验，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；循环引用检测、跨Commit依赖变更反向追踪正确。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 212 ST-FR-MV-019 — 多文件原子资源快照 — ST

- **对应要件：** FR-MV-019 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“多文件原子资源快照”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入跨租户去重存在性推断，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；断电时不会出现Commit A包含一半新素材一半未确认素材。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 213 ST-FR-MV-020 — 媒体派生缓存隔离 — ST

- **对应要件：** FR-MV-020 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“媒体派生缓存隔离”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入无法解析媒体并触发隔离限额，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；缓存污染或过期不影响原始数据，可清除并重建。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 214 ST-FR-MV-021 — 隔离媒体解析 — ST

- **对应要件：** FR-MV-021 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“隔离媒体解析”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Adapter升级、能力降级与许可证失败，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；构造ZIP炸弹/格式畸形样本，Worker触发限额时主服务不崩溃。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 215 ST-FR-MV-022 — 媒体权限和分享 — ST

- **对应要件：** FR-MV-022 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“媒体权限和分享”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入三方merge冲突与并行修改，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；撤权后旧预览URL及Range不能继续访问受保护源。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 216 ST-FR-MV-023 — 资产搜索索引 — ST

- **对应要件：** FR-MV-023 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“资产搜索索引”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入S3物理版本变化及迁移Cutover，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；权限过滤正确且展示index watermark，允许索引延迟但不伪造提交状态。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 217 ST-FR-MV-024 — 冲突可视化 — ST

- **对应要件：** FR-MV-024 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“冲突可视化”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入GC/回滚和已ACK资产的并发可达性，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；不支持内容显示元数据差异、哈希和人工冲突入口。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 218 ST-FR-MV-025 — 作品来源与授权 — ST

- **对应要件：** FR-MV-025 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“作品来源与授权”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Chunk大小边界与文件前段插入，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；授权过期的外部媒体阻断发布，下载与审计保留可授权访问。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 219 ST-FR-MV-026 — 可验证媒体导出 — ST

- **对应要件：** FR-MV-026 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“可验证媒体导出”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入篡改Chunk/Manifest与重试校验，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；导出的包在离线验证工具中重建SHA与版本谱系一致。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 220 ST-FR-MV-027 — 存储逻辑与业务版本隔离 — ST

- **对应要件：** FR-MV-027 | 阶段 E0 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“存储逻辑与业务版本隔离”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入跨租户去重存在性推断，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；迁移后GitGit revision不改变，即使目标Provider versionId完全不同。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 221 ST-FR-MV-028 — 安全垃圾回收 — ST

- **对应要件：** FR-MV-028 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“安全垃圾回收”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入无法解析媒体并触发隔离限额，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；在并发checkout/restore与GC竞态中绝不删仍有引用的Chunk。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 222 ST-FR-MV-029 — 块级迁移 — ST

- **对应要件：** FR-MV-029 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 由业务角色从客户端执行“块级迁移”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Adapter升级、能力降级与许可证失败，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；切换期间所有受保留Commit可恢复，迁移后块SHA不变。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 223 ST-FR-MV-030 — 媒体协作事件 — ST

- **对应要件：** FR-MV-030 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“媒体协作事件”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入三方merge冲突与并行修改，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；单资产事件乱序处理后最终timeline与权威Commit序列一致。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 224 ST-FR-MV-031 — 批量导入与目录映射 — ST

- **对应要件：** FR-MV-031 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“批量导入与目录映射”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入S3物理版本变化及迁移Cutover，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；导入重试不会重复资产；目录相对路径和大小写冲突按策略报告。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 225 ST-FR-MV-032 — 面向生产的预览代理 — ST

- **对应要件：** FR-MV-032 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“面向生产的预览代理”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入GC/回滚和已ACK资产的并发可达性，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；代理生成失败不阻止GitGit Checkout；展示派生版本与处理参数。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 226 ST-FR-MV-033 — 复杂工程资源类型 — ST

- **对应要件：** FR-MV-033 | 阶段 E2 | 优先级 SHOULD
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“复杂工程资源类型”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入Chunk大小边界与文件前段插入，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；缺少某Adapter不影响完整二进制保存，依赖分析显式标Unsupported。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 227 ST-FR-MV-034 — 资产语义标签 — ST

- **对应要件：** FR-MV-034 | 阶段 E1 | 优先级 MUST
- **前提：** 隔离完整产品部署、Web/CLI/真实媒体样本/可选S3 Provider；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 由业务角色从客户端执行“资产语义标签”完整流程，记录输入SHA和GitGit commit；2) 在真实负载下重复/恢复历史媒体及对比资产；3) 注入篡改Chunk/Manifest与重试校验，观察用户可见错误与回滚/恢复；4) 审查权限、审计证据、最终内容全文件SHA和资源引用。
- **判定：** 业务流程符合功能要件接受条件，数据可恢复且失败明确提示；可查询某Commit下标签快照，后改标签不篡改提交历史。
- **证据：** client recording + GitGit Commit/Tree + complete byte hashes + operational metrics + audit
- **方法：** E2E scenario/chaos (manual signoff)
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 228 ST-NFR-GG-001 — 100TB规模恢复一致性— NFR定量/质量验收

- **对应要件：** NFR-GG-001 | 阶段 E1 | 优先级 MUST
- **前提：** 100TB逻辑媒体仓库、跨存储迁移、从历史任意Commit恢复；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：100TB逻辑媒体仓库、跨存储迁移、从历史任意Commit恢复；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 0块SHA不一致；不丢已ACK提交；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 229 ST-NFR-GG-002 — 版本图可追溯— NFR定量/质量验收

- **对应要件：** NFR-GG-002 | 阶段 E1 | 优先级 MUST
- **前提：** 1000万Revision关联不同Commit/Branch/Tag；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：1000万Revision关联不同Commit/Branch/Tag；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 全历史可恢复；无悬空边和循环；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 230 ST-NFR-GG-003 — 块级增量传输收益— NFR定量/质量验收

- **对应要件：** NFR-GG-003 | 阶段 E1 | 优先级 MUST
- **前提：** 指定1GiB构造数据前部插入1KiB；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：指定1GiB构造数据前部插入1KiB；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 传输实际缺失Chunk；实验报告字节比及固定分块对照，不固定跨格式承诺；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 231 ST-NFR-GG-004 — 大媒体常数级内存— NFR定量/质量验收

- **对应要件：** NFR-GG-004 | 阶段 E1 | 优先级 MUST
- **前提：** 20GiB原件上传/Range/下载及resume；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：20GiB原件上传/Range/下载及resume；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 网关单请求峰值堆内存≤约定阈值且不随文件大小线性增长；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 232 ST-NFR-GG-005 — 资产历史查询— NFR定量/质量验收

- **对应要件：** NFR-GG-005 | 阶段 E1 | 优先级 MUST
- **前提：** 千万级Revision样本；热缓存与冷查询；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：千万级Revision样本；热缓存与冷查询；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 基线下p95阈值待SLO合意、提供原始分布；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 233 ST-NFR-GG-006 — 提交与内容强一致— NFR定量/质量验收

- **对应要件：** NFR-GG-006 | 阶段 E1 | 优先级 MUST
- **前提：** Ref CAS、元数据Leader failover、Chunk落盘断电；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：Ref CAS、元数据Leader failover、Chunk落盘断电；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** ACK后内容可还原；缺块不发布新Ref；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 234 ST-NFR-GG-007 — 离线冲突零静默丢失— NFR定量/质量验收

- **对应要件：** NFR-GG-007 | 阶段 E1 | 优先级 MUST
- **前提：** 两个工作区同一大文件并发离线修改；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：两个工作区同一大文件并发离线修改；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 无未提示自动覆盖，存在可审计决议；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 235 ST-NFR-GG-008 — 媒体插件沙盒边界— NFR定量/质量验收

- **对应要件：** NFR-GG-008 | 阶段 E1 | 优先级 MUST
- **前提：** 解析畸形文件、逃逸试探、过载媒体Worker；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：解析畸形文件、逃逸试探、过载媒体Worker；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 核心存储无密钥泄露和代码执行；资源耗尽受限；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 236 ST-NFR-GG-009 — 细粒度撤权— NFR定量/质量验收

- **对应要件：** NFR-GG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 已签发媒体播放、Range和预览令牌后撤权；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：已签发媒体播放、Range和预览令牌后撤权；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 撤权传播边界经约定；强制访问点不能绕过授权；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 237 ST-NFR-GG-010 — 格式适配器能力清单— NFR定量/质量验收

- **对应要件：** NFR-GG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 新增/禁用/升级媒体解析与差异插件；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：新增/禁用/升级媒体解析与差异插件；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 未支持能力明确降级；不破坏既存Manifest/Revision；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 238 ST-NFR-GG-011 — 供应链许可证合规— NFR定量/质量验收

- **对应要件：** NFR-GG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 所有Rust crate、JS包、二进制Worker构建；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：所有Rust crate、JS包、二进制Worker构建；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** SBOM覆盖全部发布产物；引入强Copyleft/非自由组件需阻断或批准隔离；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 239 ST-NFR-GG-012 — 数据格式可移植— NFR定量/质量验收

- **对应要件：** NFR-GG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 原生Commit/Manifest/Chunk导出与重建；记录受控硬件/网络/版本/数据集/合意阈值
- **测试步骤：** 1) 固定真实集群/负载/数据样本及GitGit与MinIOS3版本；2) 运行场景：原生Commit/Manifest/Chunk导出与重建；3) 收集每次调用耗时/失败率/原始哈希/安全审计与机器资源；4) 对照签署SLO/安全预期并复算；5) 进行恢复/故障注入和二次复验。
- **判定：** 不同存储后端/系统版本离线校验可恢复完整媒体；未签署或未达到目标则Blocked/Fail，不得视为Pass。
- **证据：** raw load samples + SHA manifest + environment inventory + audit & signed decision
- **方法：** ST load/chaos/security
- **执行结果：** Not Run
- **阻断说明：** 部分NFR阈值与部署环境待业务签署

## 240 ST-UC-GG-01 — 业务端到端 — 修改10GiB视频并提交GitGit分支

- **对应要件：** FR-GG-001, FR-MV-004, FR-MV-007 | 阶段 E1 | 优先级 MUST
- **前提：** 由美术师扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：修改10GiB视频并提交GitGit分支；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 增量上传缺失块、签名确认、提交后可还原全部字节；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 241 ST-UC-GG-02 — 业务端到端 — 比较两个配音版本

- **对应要件：** FR-MV-015, FR-MV-024 | 阶段 E1 | 优先级 MUST
- **前提：** 由声音制作扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：比较两个配音版本；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 波形、区间和声道差异定位，未知格式安全降级；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 242 ST-UC-GG-03 — 业务端到端 — 比较模型材质/网格/动画改动

- **对应要件：** FR-MV-016, FR-MV-018 | 阶段 E1 | 优先级 MUST
- **前提：** 由3D美术扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：比较模型材质/网格/动画改动；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 3D差异可定位、关键变化具备来源证明；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 243 ST-UC-GG-04 — 业务端到端 — 修改贴图影响多个模型

- **对应要件：** FR-MV-018, FR-GG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 由游戏团队扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：修改贴图影响多个模型；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 依赖图定位所有受影响的AssetRevision；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 244 ST-UC-GG-05 — 业务端到端 — 创建分支、并行修改并合并

- **对应要件：** FR-GG-003, FR-GG-012, FR-GG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 由策划扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：创建分支、并行修改并合并；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 有冲突时禁止静默覆盖，结果含审批/决议记录；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 245 ST-UC-GG-06 — 业务端到端 — 离线编辑并按需同步媒体

- **对应要件：** FR-GG-010, FR-GG-011, FR-MV-007 | 阶段 E1 | 优先级 MUST
- **前提：** 由移动办公者扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：离线编辑并按需同步媒体；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 冲突显式提示、未修改块不重复上传；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 246 ST-UC-GG-07 — 业务端到端 — 从一种S3后端切换并恢复历史仓库

- **对应要件：** FR-MV-027, FR-MV-029, FR-GG-020 | 阶段 E1 | 优先级 MUST
- **前提：** 由管理员扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：从一种S3后端切换并恢复历史仓库；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 所有受保留Commit完整检出且S3物理版本迁移不影响GitGit ID；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 247 ST-UC-GG-08 — 业务端到端 — 阻断不受信插件读取保密视频

- **对应要件：** FR-MV-021, NFR-GG-008 | 阶段 E1 | 优先级 MUST
- **前提：** 由安全审计扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：阻断不受信插件读取保密视频；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 隔离Worker无权限，拒绝详情审计可查；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 248 ST-UC-GG-09 — 业务端到端 — 审批多个媒体资产后发布版本

- **对应要件：** FR-GG-014, FR-MV-025 | 阶段 E1 | 优先级 MUST
- **前提：** 由主美扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：审批多个媒体资产后发布版本；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 未审批资源阻断受保护分支发布，审批记录不可篡改；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run

## 249 ST-UC-GG-10 — 业务端到端 — 误删分支并发执行GC后恢复

- **对应要件：** FR-GG-016, FR-MV-028 | 阶段 E1 | 优先级 MUST
- **前提：** 由运维扮演真实使用者；准备至少两个媒体版本、两种权限角色、前后版本SHA、隔离租户与仓库
- **测试步骤：** 1) 登录真实GitGit+MinIOS3界面/CLI；2) 执行业务任务：误删分支并发执行GC后恢复；3) 针对相同资产创建冲突/中断/非法授权负例；4) 将最终Commit、Manifest和物理对象的SHA逐一比对并收集审计与用户可见证据；5) 业务Owner对完成条件签署。
- **判定：** 受保护历史引用内容未被提前删除；业务要求和所有关联需求的权限/数据完整性约束均符合。
- **证据：** business sign-off + video/screenshots + SHA digest + traces + incident record
- **方法：** ST E2E/UAT
- **执行结果：** Not Run
- **阻断说明：** 真实GitGit Native/多媒体UI及扩展格式实现前Not Run
