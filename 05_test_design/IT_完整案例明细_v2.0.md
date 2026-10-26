# IT 完整测试案例明细 — GitGit + MinIOS3 v2.0

> 共 194条设计案例 | Not Run | 所有Case ID唯一；旧案例沿用编号，新业务主链路按原生GitGit验收。

## 001 IT-FR-CORE-001 — 独立部署-组件协议联调

- **对应要件：** FR-CORE-001 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 验证新要求：断开公网后，GitGit原生提交、版本回退、媒体分块上传/下载、MinIOS3 PUT/GET/LIST和管理可正常完成；禁用LFS适配仍通过。；关闭Git LFS兼容适配时原生媒体版本提交仍必须可用。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 002 IT-FR-CORE-002 — Native 与 Federation 运行模式-组件协议联调

- **对应要件：** FR-CORE-002 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：两种部署模式均通过 S3 核心兼容测试；模式切换不得改变现有对象内容。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 003 IT-FR-CORE-003 — 租户层级-组件协议联调

- **对应要件：** FR-CORE-003 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：租户 A 凭据无法枚举或读取租户 B 的任何资源；隔离自动测试100%通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 004 IT-FR-CORE-004 — 租户资源计量和配额-组件协议联调

- **对应要件：** FR-CORE-004 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：超出硬配额返回明确错误，管理员可追踪来源；计量误差可核对。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 005 IT-FR-CORE-005 — 存储命名空间和资源映射-组件协议联调

- **对应要件：** FR-CORE-005 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：迁移后使用原端点、原 Bucket/Key 可以正确读取原版本。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 006 IT-FR-CORE-006 — 配置版本与回滚-组件协议联调

- **对应要件：** FR-CORE-006 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：错误路由配置可恢复上一生效版本，不改变已确认对象。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 007 IT-FR-CORE-007 — 多区域和数据驻留策略-组件协议联调

- **对应要件：** FR-CORE-007 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：禁止区域间转移时，对应迁移/复制任务无法启动并写入审计记录。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 008 IT-FR-CORE-008 — 能力发现接口-组件协议联调

- **对应要件：** FR-CORE-008 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) 真实Rust Admin/API + metadata service联调，分别以Tenant-A和Tenant-B发送同一路径写读；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 外部返回不泄漏另一租户信息；配置变化对应单个目录revision；逐项验收：API和UI可取得对应版本的能力清单；不支持功能明确返回非兼容错误。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 009 IT-FR-S3-001 — Bucket 生命周期-组件协议联调

- **对应要件：** FR-S3-001 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：创建、列举、空桶删除、权限错误及地域配置测试通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 010 IT-FR-S3-002 — 标准对象读写-组件协议联调

- **对应要件：** FR-S3-002 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：主流 S3 SDK 的上传、下载、覆盖、条件请求符合兼容基线。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 011 IT-FR-S3-003 — 对象列举-组件协议联调

- **对应要件：** FR-S3-003 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 预置 Key [a,a/1,a//b,汉字,emoji], prefix/delimiter筛选；同时插入100新key并用同一快照token翻页；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 每页最多max-keys，字节排序无重复无遗漏；篡改token/跨tenant token拒绝；对照原要件验收：千万级键空间分页不漏不重；与提交确认的写入符合指定一致性。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** REV-010:跨range强LIST未证明前只测单Bucket单Group

## 012 IT-FR-S3-004 — 分段上传-组件协议联调

- **对应要件：** FR-S3-004 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) CreateMultipart；乱序上传Part 3/1/2；并发Complete与Abort；对已提交session重试并断开HTTP；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 只有一次ObjectVersion publish，parts排序准确；未发布part对GET不可见；对照原要件验收：分段乱序、断线续传、重复Complete、Abort及并发重试通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 013 IT-FR-S3-005 — 版本控制-组件协议联调

- **对应要件：** FR-S3-005 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 在NeverEnabled→Enabled→Suspended→Enabled下依次PUT/DELETE含显式VersionId、null及DeleteMarker；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** VersionId稳定，旧版合法GET，锁定版本不可删除，DeleteMarker符合兼容矩阵；对照原要件验收：覆盖与删除后，历史版本可依授权恢复，版本标识稳定且唯一。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 014 IT-FR-S3-006 — 签名鉴权-组件协议联调

- **对应要件：** FR-S3-006 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 准备已知AWS SigV4 canonical向量，依次修改Host、%2F、SignedHeaders、X-Amz-Date、payload bytes；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 签名任何参与字段被改即403，TLS代理未修改原始Host/Path时合法签名成功；对照原要件验收：官方AWS SDK认证场景及坏签名、过期时间戳均有正确响应。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 015 IT-FR-S3-007 — 临时预签名-组件协议联调

- **对应要件：** FR-S3-007 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：过期、越权、撤销后无法访问；范围约束与审计关联正确。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 016 IT-FR-S3-008 — HTTP Range 与流式传输-组件协议联调

- **对应要件：** FR-S3-008 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：对≥20GiB对象进行Range读取，不整体加载内存，返回正确206与校验。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 017 IT-FR-S3-009 — 对象属性及标签-组件协议联调

- **对应要件：** FR-S3-009 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：元数据/标签在复制迁移后按策略完整保留。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 018 IT-FR-S3-010 — 分层策略-组件协议联调

- **对应要件：** FR-S3-010 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：默认拒绝、多租户隔离、策略冲突和SDK常见操作矩阵通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 019 IT-FR-S3-011 — 生命周期管理-组件协议联调

- **对应要件：** FR-S3-011 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：TTL到期任务正确执行；被保留/保护的版本不被误删。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 020 IT-FR-S3-012 — Object Lock-组件协议联调

- **对应要件：** FR-S3-012 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 旧Version设Governance和Compliance，试Governance绕过、有无权限、缩短Compliance、LegalHold解除、时钟回拨；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Compliance始终拒绝提前物理删除；新Version/合法DeleteMarker按协议允许；对照原要件验收：WORM对覆盖、版本删除、撤销保留、时钟回拨等攻击测试拒绝；独立审计完成。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 021 IT-FR-S3-013 — 事件通知-组件协议联调

- **对应要件：** FR-S3-013 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：消费者重复收到事件也能幂等处理；失败可重放。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 022 IT-FR-S3-014 — SSE/KMS兼容-组件协议联调

- **对应要件：** FR-S3-014 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：使用配置KMS读写正常，密钥拒绝/轮换行为受测。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 023 IT-FR-S3-015 — Object Checksum-组件协议联调

- **对应要件：** FR-S3-015 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：篡改块校验失败；迁移前后SHA-256一致。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 024 IT-FR-S3-016 — S3 Batch 操作-组件协议联调

- **对应要件：** FR-S3-016 | 阶段 E2 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：10万对象批量任务中断重启不丢状态、无重复破坏性变更。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 025 IT-FR-S3-017 — S3 兼容性清单-组件协议联调

- **对应要件：** FR-S3-017 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：发布版本附机器可读兼容清单，并对宣称支持的API自动回归。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 026 IT-FR-S3-018 — 虚拟主机与路径风格-组件协议联调

- **对应要件：** FR-S3-018 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：两种地址形式在合法桶名、特殊字符、代理TLS下通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 027 IT-FR-S3-019 — 对象幂等与并发条件-组件协议联调

- **对应要件：** FR-S3-019 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) AWS CLI + boto3 + AWS SDK Rust + JS SDK至少各一条真实签名请求，校对返回头/XML/字节和元数据；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 宣称支持的API与兼容矩阵结果一致；异常返回标准S3协议；逐项验收：重试不会生成损坏版本；并发相同键操作呈现可解释顺序。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 028 IT-FR-GIT-001 — [v2修订] Git LFS Batch API - IT验证

- **对应要件：** FR-GIT-001 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：LFS兼容客户端传输与SHA校验通过；禁用LFS兼容服务不影响GitGit原生E0媒体版本链路。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 029 IT-FR-GIT-002 — [v2修订] Git LFS Locking API - IT验证

- **对应要件：** FR-GIT-002 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：GitGit锁与LFS锁查询一致；未启用LFS时原生协作锁仍有效。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 030 IT-FR-GIT-003 — [v2修订] 标准Git独立性 - IT验证

- **对应要件：** FR-GIT-003 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：关闭标准Git互操作插件后GitGit原生版本管理仍能提交、分支、比较和恢复。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 031 IT-FR-GIT-004 — [v2修订] 内容寻址 - IT验证

- **对应要件：** FR-GIT-004 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：同字节同策略域复用且不改变原件；租户隔离，读取重新校验。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 032 IT-FR-GIT-005 — [v2修订] 引用完整性检查 - IT验证

- **对应要件：** FR-GIT-005 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：缺块、旧epoch、非法ACL或过期票据均拒绝提交；旧Ref保持不变。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 033 IT-FR-GIT-006 — [v2修订] 版本关联索引 - IT验证

- **对应要件：** FR-GIT-006 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：Commit→资产→内容块→物理存放可逆向追踪且hash验证一致。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 034 IT-FR-GIT-007 — [v2修订] Git分支/Tag与媒体历史 - IT验证

- **对应要件：** FR-GIT-007 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：创建、切换、比较、回退GitGit分支正确还原媒体内容；S3版本ID差异不影响历史。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 035 IT-FR-GIT-008 — [v2修订] Git身份继承 - IT验证

- **对应要件：** FR-GIT-008 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：撤销GitGit权限后新的原生读取及签名URL请求被拒；跨租户不可枚举CAS。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 036 IT-FR-GIT-009 — [v2修订] Git事件接入 - IT验证

- **对应要件：** FR-GIT-009 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：重复、乱序、遗漏后通过快照watermark对账；GC对未证完整可达性拒删。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 037 IT-FR-GIT-010 — [v2修订] 媒体垃圾回收安全 - IT验证

- **对应要件：** FR-GIT-010 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：在并发新引用、迁移、快照与合法保留期间零误删。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 038 IT-FR-GIT-011 — [v2修订] 离线和部分克隆 - IT验证

- **对应要件：** FR-GIT-011 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：离线提交后重连按冲突策略重新校验；只下载请求Chunk并保持SHA一致。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 039 IT-FR-GIT-012 — 仓库配额和限流-组件协议联调

- **对应要件：** FR-GIT-012 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff；2) git-lfs CLI通过Git Server测试repo完成Batch/Lock/push/fetch，Git Server伪造和丢失Webhook再对账；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Git Ref提交前检查有效；下载按RepoGrant隔离；重复事件不重复grant；逐项验收：超配额上传在写入前可预测拒绝，已存在历史不被误删。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 040 IT-FR-GIT-013 — [v2修订] 提交一致性补偿 - IT验证

- **对应要件：** FR-GIT-013 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：断电、超时、重复请求不会产生可见缺块提交；补偿可恢复且GC保护。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 041 IT-FR-GIT-014 — [v2修订] 现有Git迁移 - IT验证

- **对应要件：** FR-GIT-014 | 阶段 E2 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：导入后保留历史关系与内容摘要且输出无法映射特性的差异报告。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 042 IT-FR-MEDIA-001 — 多媒体资产分类-组件协议联调

- **对应要件：** FR-MEDIA-001 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：至少PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP完成端到端验证。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 043 IT-FR-MEDIA-002 — 资产元数据-组件协议联调

- **对应要件：** FR-MEDIA-002 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：元数据与Git Commit及OID可以双向检索。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 044 IT-FR-MEDIA-003 — 内容零改写-组件协议联调

- **对应要件：** FR-MEDIA-003 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：下载SHA-256与原上传文件相同。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 045 IT-FR-MEDIA-004 — 异步预览-组件协议联调

- **对应要件：** FR-MEDIA-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：预览出错时下载与版本读取不受影响，且有失败原因。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 046 IT-FR-MEDIA-005 — [v2修订] 版本差异辅助 - IT验证

- **对应要件：** FR-MEDIA-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：通过媒体专用golden fixture识别变化，无法识别时回退hash/size而不宣称语义相等。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 047 IT-FR-MEDIA-006 — 资产搜索-组件协议联调

- **对应要件：** FR-MEDIA-006 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：指定索引规模下按分页检索，结果遵循权限隔离。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 048 IT-FR-MEDIA-007 — 媒体预览安全-组件协议联调

- **对应要件：** FR-MEDIA-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：构造异常媒体文件不能访问宿主机密钥或其他租户对象。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 049 IT-FR-MEDIA-008 — 预览内容与源关联-组件协议联调

- **对应要件：** FR-MEDIA-008 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：源变化触发可识别的重新生成；源内容保持不可变。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 050 IT-FR-MEDIA-009 — 媒体交付-组件协议联调

- **对应要件：** FR-MEDIA-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：断网后续传与视频Seek测试通过；缓存不泄漏租户数据。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 051 IT-FR-MEDIA-010 — 许可与溯源-组件协议联调

- **对应要件：** FR-MEDIA-010 | 阶段 E2 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 上传真实媒体fixture，派生Worker及媒体索引服务回调，带租户身份查询/下载原件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源内容SHA一致；异步预览可失败/重试，索引水位清楚；逐项验收：受限制素材无法被标记为可公开发布。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 052 IT-FR-XCHG-001 — S3后端适配-组件协议联调

- **对应要件：** FR-XCHG-001 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：同一测试数据在至少AWS S3与另一个自建S3实现上完整往返。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 053 IT-FR-XCHG-002 — 非S3供应商适配-组件协议联调

- **对应要件：** FR-XCHG-002 | 阶段 E2 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：增补适配器无需更改S3客户端公开接口。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 054 IT-FR-XCHG-003 — 统一端点-组件协议联调

- **对应要件：** FR-XCHG-003 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：原有客户端仅改Endpoint/密钥即可访问受支持S3 API。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 055 IT-FR-XCHG-004 — 后端能力矩阵-组件协议联调

- **对应要件：** FR-XCHG-004 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：要求WORM而后端不满足时禁止部署，不允许虚假成功。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 056 IT-FR-XCHG-005 — 存储策略与放置-组件协议联调

- **对应要件：** FR-XCHG-005 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：Dry-run输出每条规则的匹配原因和成本估计。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 057 IT-FR-XCHG-006 — 迁移作业-组件协议联调

- **对应要件：** FR-XCHG-006 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 两个Provider对同一版本赋予不同physical VersionId，抽取旧版/delete marker/metadata/Tag/Lock后杀死Worker；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Checkpoint重放不漏Version；public逻辑VersionId保持，源/目标SHA及length一致；对照原要件验收：百万对象迁移中断后恢复，不漏文件；核验摘要与元数据。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 058 IT-FR-XCHG-007 — 迁移预演-组件协议联调

- **对应要件：** FR-XCHG-007 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：存在不兼容Object Lock时必须报阻断项。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 059 IT-FR-XCHG-008 — 无损切换-组件协议联调

- **对应要件：** FR-XCHG-008 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 完成Cutover后在新主写入V2并收到ACK，随后模拟目标故障请求立即回旧源；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 旧源未追平时必须阻断/只读；反向追平验证后才能CAS切回，V2仍可GET；对照原要件验收：生产演练迁移失败可回退到源端，用户已确认写入不丢失。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 060 IT-FR-XCHG-009 — 双端校验-组件协议联调

- **对应要件：** FR-XCHG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 人为损坏目标某个历史Version和Tag，执行完整Verification；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 分别输出HashMismatch和MetadataMismatch，任何差异阻断Cutover；对照原要件验收：对人为损坏和缺失分别准确告警。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 061 IT-FR-XCHG-010 — 连续复制-组件协议联调

- **对应要件：** FR-XCHG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：乱序和网络分区不产生静默覆盖；复制滞后指标可见。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 062 IT-FR-XCHG-011 — 冲突控制-组件协议联调

- **对应要件：** FR-XCHG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：并发写同键情况下不允许最后写入静默掩盖冲突。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 063 IT-FR-XCHG-012 — 读取故障转移-组件协议联调

- **对应要件：** FR-XCHG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：故障转移返回一致性状态；未同步数据不得误报存在。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 064 IT-FR-XCHG-013 — 后端出口开放-组件协议联调

- **对应要件：** FR-XCHG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：第三方工具可从导出的数据重建完整键空间及版本关系。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 065 IT-FR-XCHG-014 — 成本与计量-组件协议联调

- **对应要件：** FR-XCHG-014 | 阶段 E2 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：成本报表能与供应商账单进行差额核对。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 066 IT-FR-XCHG-015 — 底层供应商异常隔离-组件协议联调

- **对应要件：** FR-XCHG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 通过统一Endpoint同时接AWS与另一自建S3，进行有历史版本对象的Dry-run/受控迁移；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 所有版本含DeleteMarker映射正确；阻断项阻止切换，导出可恢复；逐项验收：一个后端故障不会拖垮其它桶的正常请求。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 067 IT-FR-NATIVE-001 — 多节点横向扩展-组件协议联调

- **对应要件：** FR-NATIVE-001 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：扩容演练不需要停掉全部S3服务。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 068 IT-FR-NATIVE-002 — 纠删码/数据保护-组件协议联调

- **对应要件：** FR-NATIVE-002 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 依次验证6/6、5/6、4/6 durable receipt；在PUT步骤P2/P3/P4断电/断网；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 仅6片达标且Raft Commit后可ACK；其他进入orphan/unknown结果并可补偿；对照原要件验收：配置容错范围内故障可读取完整数据，超出范围进入安全只读/错误状态。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 069 IT-FR-NATIVE-003 — 故障域意识-组件协议联调

- **对应要件：** FR-NATIVE-003 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：放置审计证明碎片满足故障域策略。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 070 IT-FR-NATIVE-004 — 节点自愈与巡检-组件协议联调

- **对应要件：** FR-NATIVE-004 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：混合故障注入后数据摘要不变，进度和剩余风险可查询。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 071 IT-FR-NATIVE-005 — 存储一致性语义-组件协议联调

- **对应要件：** FR-NATIVE-005 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 对同Key并发覆盖/删/GET/HEAD/LIST并在leader切换和快照压缩时执行；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK后读一致、无半对象；单Bucket单Group，跨range模式未证明前disabled；对照原要件验收：ACK后GET/HEAD/LIST（在规定作用域）立即可见，不出现半对象。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** REV-010:跨range强LIST未证明前只测单Bucket单Group

## 072 IT-FR-NATIVE-006 — 数据与控制面故障隔离-组件协议联调

- **对应要件：** FR-NATIVE-006 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：单点注入与恢复试验通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 073 IT-FR-NATIVE-007 — 容量水位保护-组件协议联调

- **对应要件：** FR-NATIVE-007 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：容量接近上限能预测并安全拒绝写入。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 074 IT-FR-NATIVE-008 — 数据损坏检测-组件协议联调

- **对应要件：** FR-NATIVE-008 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：注入存储位错误后检测到并在冗余足够时自愈。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 075 IT-FR-NATIVE-009 — 故障演练接口-组件协议联调

- **对应要件：** FR-NATIVE-009 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 真实meta quorum和至少6个存储节点，执行PUT/GET、故障盘、重启、repair；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK由Data Receipts+Meta Commit支撑；故障超过保护能力明确报错；逐项验收：每个发布候选版本执行故障注入测试。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 076 IT-FR-SEC-001 — SSO与身份联邦-组件协议联调

- **对应要件：** FR-SEC-001 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：SSO登录/登出/禁用账号后会话生效性经测试。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 077 IT-FR-SEC-002 — IAM/RBAC/ABAC-组件协议联调

- **对应要件：** FR-SEC-002 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：尝试越权访问、横向提权和策略覆盖均失败。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 078 IT-FR-SEC-003 — 短期凭据-组件协议联调

- **对应要件：** FR-SEC-003 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：过期Token和已撤销AccessKey被拒绝并审计。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 079 IT-FR-SEC-004 — 传输加密-组件协议联调

- **对应要件：** FR-SEC-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：未加密入口默认关闭；TLS配置可扫描验证。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 080 IT-FR-SEC-005 — 静态加密与密钥管理-组件协议联调

- **对应要件：** FR-SEC-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 在KMS revoke/轮换/timeout期间上传读历史Version，变更DEK wrapper；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不明文回退，旧版授权读仍可验证或明确拒绝；审计完整；对照原要件验收：KMS故障不会造成明文回退，轮换后的旧数据可访问。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 081 IT-FR-SEC-006 — 审计追踪-组件协议联调

- **对应要件：** FR-SEC-006 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：追踪一个Git Commit至对应OID和S3 Version与访问日志。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 082 IT-FR-SEC-007 — 不可篡改审计-组件协议联调

- **对应要件：** FR-SEC-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 阻断外部审计Sink 30min、重启outbox sender、篡改Sink中历史记录；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 日志队列不丢且事件去重；篡改hash链告警；普通Ops无法重写；对照原要件验收：系统管理员不能通过普通运维权限覆盖已封存审计记录。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 083 IT-FR-SEC-008 — 恶意软件与内容控制-组件协议联调

- **对应要件：** FR-SEC-008 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：恶意媒体可隔离并可追查，原始SHA-256和隔离动作有证据。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 084 IT-FR-SEC-009 — 管理面防护-组件协议联调

- **对应要件：** FR-SEC-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：删除存储池、强制解锁、迁移切换触发审批规则。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 085 IT-FR-SEC-010 — 安全加固与供应链-组件协议联调

- **对应要件：** FR-SEC-010 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：关键漏洞阻止正式发布，SBOM可下载。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 086 IT-FR-SEC-011 — 合法持有与数据隔离-组件协议联调

- **对应要件：** FR-SEC-011 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：租户删除前检测WORM和Git活跃引用，依法合规执行。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 087 IT-FR-SEC-012 — 秘密管理-组件协议联调

- **对应要件：** FR-SEC-012 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：数据导出和日志不包含明文AccessKey/SecretKey。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 088 IT-FR-SEC-013 — 勒索防护-组件协议联调

- **对应要件：** FR-SEC-013 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：批量误删后可以按保留策略恢复；模拟攻击产生报警。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 089 IT-FR-SEC-014 — 合规声明边界-组件协议联调

- **对应要件：** FR-SEC-014 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) IdP/KMS/审计sink与Gateway联调，覆盖撤权、STS过期、Retention和锁保护；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不存在明文fallback/越权/非法删Version，审计outbox可重播；逐项验收：发布材料包含适用范围、例外、证据和未认证声明。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 090 IT-FR-OPS-001 — Web管理控制台-组件协议联调

- **对应要件：** FR-OPS-001 | 阶段 E0 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：任务列表和对象详情三步内可进入，角色不可见不属于自己的资源。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 091 IT-FR-OPS-002 — 版本关系可视化-组件协议联调

- **对应要件：** FR-OPS-002 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：定位一项历史资源无需理解后端私有路径。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 092 IT-FR-OPS-003 — 迁移与复制工作台-组件协议联调

- **对应要件：** FR-OPS-003 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：操作可审计；危险动作有影响预览。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 093 IT-FR-OPS-004 — 媒体浏览器-组件协议联调

- **对应要件：** FR-OPS-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：图像、视频、3D预览异常不阻断下载。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 094 IT-FR-OPS-005 — 多语言-组件协议联调

- **对应要件：** FR-OPS-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：语言切换不改变权限/对象数据；日语信息自然一致。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 095 IT-FR-OPS-006 — 无障碍与认知负荷-组件协议联调

- **对应要件：** FR-OPS-006 | 阶段 E2 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：键盘完成桶创建、恢复版本和权限查询，核心流程可用性评审通过。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 096 IT-FR-OPS-007 — CLI与配置即代码-组件协议联调

- **对应要件：** FR-OPS-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：操作与Web实现相同权限与审计策略。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 097 IT-FR-OPS-008 — API文档与SDK示例-组件协议联调

- **对应要件：** FR-OPS-008 | 阶段 E2 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：新开发者按文档完成上传、Git LFS和迁移样例。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 098 IT-FR-OPS-009 — 审计查询与报告-组件协议联调

- **对应要件：** FR-OPS-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：报告可重现追查一次管理员动作与Git修改。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 099 IT-FR-OPS-010 — 监控与告警-组件协议联调

- **对应要件：** FR-OPS-010 | 阶段 E0 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：Grafana/兼容仪表板可读取关键指标并按阈值触发。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 100 IT-FR-OPS-011 — 零停机滚动升级-组件协议联调

- **对应要件：** FR-OPS-011 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) N/N-1混部滚动更替Meta voters、数据节点和Gateway，故障在Meta schema迁移中发生；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** quorum不丢、旧版能读新版本记录或进入阻断回退，备份可恢复；对照原要件验收：在允许的单节点轮替中读写持续；失败能按预案恢复。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 101 IT-FR-OPS-012 — 备份与恢复-组件协议联调

- **对应要件：** FR-OPS-012 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：仅从备份与对象数据可复原Git映射和用户可读对象。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 102 IT-FR-OPS-013 — 异常与根因信息-组件协议联调

- **对应要件：** FR-OPS-013 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：S3错误+后台trace可串联到具体后端与操作。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 103 IT-FR-OPS-014 — 运维 runbook-组件协议联调

- **对应要件：** FR-OPS-014 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：演练人员不依赖开发者介入完成预定操作。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 104 IT-FR-OPS-015 — 服务水平报告-组件协议联调

- **对应要件：** FR-OPS-015 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) Admin OpenAPI和minios3ctl、Web UI调用同一Rust backend，实现审批和Audit检索；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 授权与审批在CLI/Web一致，操作ID与审计ID可以反查；逐项验收：月报按原始监控数据复算一致。
- **证据：** HTTP/gRPC captures(redacted)+ service trace + physical hash + Git/Provider response
- **方法：** CI integration + disposable backends
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 105 IT-NFR-AVA-001 — 生产可用率-指标与质量门禁

- **对应要件：** NFR-AVA-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『E1至少3故障域、正常维护口径按SLA另行约定』下测得『月度 S3 API 可用率≥99.95%（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 106 IT-NFR-AVA-002 — 确认写入持久性-指标与质量门禁

- **对应要件：** NFR-AVA-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任意单节点/单磁盘故障及配置允许的故障数量范围内』下测得『已返回2xx的对象不得丢失，损坏可自动检出』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 107 IT-NFR-AVA-003 — 站点恢复-指标与质量门禁

- **对应要件：** NFR-AVA-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『异地主备配置已启用、依赖服务可用』下测得『生产RTO≤60分钟；异步跨站RPO≤15分钟（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 108 IT-NFR-AVA-004 — 同站数据一致性-指标与质量门禁

- **对应要件：** NFR-AVA-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『针对primary、ACK后单键与LIST可见性』下测得『ACK后GET/HEAD/LIST强一致；跨站异步复制单独声明』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 109 IT-NFR-AVA-005 — WORM恢复安全-指标与质量门禁

- **对应要件：** NFR-AVA-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『磁盘失效、节点回滚、时钟漂移』下测得『保留版本在保留期限内不得通过应用API提前删除』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 110 IT-NFR-AVA-006 — 恢复演练周期-指标与质量门禁

- **对应要件：** NFR-AVA-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产变更与季度计划』下测得『季度DR演练；每月备份恢复抽样验证』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 111 IT-NFR-MIG-001 — 迁移数据准确度-指标与质量门禁

- **对应要件：** NFR-MIG-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『≥100万对象，含非当前版本、标签、锁定对象』下测得『对象SHA-256及字节长度 100%一致；元数据差异全部列明』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 112 IT-NFR-MIG-002 — 迁移中断恢复-指标与质量门禁

- **对应要件：** NFR-MIG-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『随机kill迁移进程和目标后端5xx』下测得『重启后接续、不出现漏项；无法迁移对象附异常报告』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 113 IT-NFR-MIG-003 — 迁移回滚-指标与质量门禁

- **对应要件：** NFR-MIG-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『源站保留至独立验证和审批完成』下测得『失败切换可逆且不会覆盖源站已确认写入』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 114 IT-NFR-MIG-004 — 外部兼容性-指标与质量门禁

- **对应要件：** NFR-MIG-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照
- **测试步骤：** 1) AWS S3+另一自建兼容后端、100万对象含历史/Retention；保留源端不可变快照；2) 源目标版本完整扫描与SHA/length 100%检查，迁移过程中随机Kill、429、5xx与cutover rollback；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『AWS CLI、aws-sdk-js v3、boto3、Go/Rust S3 SDK、git-lfs』下测得『宣称兼容功能通过协议回归测试≥99%；0个阻断级差异』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 115 IT-NFR-SEC-001 — 权限拒绝-指标与质量门禁

- **对应要件：** NFR-SEC-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任意匿名、过期、跨租户、撤销凭据』下测得『敏感资源默认拒绝；跨租户访问成功数=0』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 116 IT-NFR-SEC-002 — 加密-指标与质量门禁

- **对应要件：** NFR-SEC-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产外部/东西向连接、静态数据』下测得『TLS1.2+；静态强加密并依赖受控KMS；禁用明文后退』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 117 IT-NFR-SEC-003 — 凭据处理-指标与质量门禁

- **对应要件：** NFR-SEC-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『CI/CD、日志、控制台、事故现场』下测得『不能输出密钥明文；密钥轮换和吊销符合审计要求』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 118 IT-NFR-SEC-004 — 漏洞要求-指标与质量门禁

- **对应要件：** NFR-SEC-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『正式版本发布前及修复SLAs』下测得『0个已知未接受的Critical漏洞；高危经审批例外』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 119 IT-NFR-SEC-005 — 审计保留-指标与质量门禁

- **对应要件：** NFR-SEC-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『关键安全日志含Object Lock/KMS/Git操作』下测得『审计事件可关联且在外部不可变存储受控保留≥365天（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 120 IT-NFR-SEC-006 — 供应链验证-指标与质量门禁

- **对应要件：** NFR-SEC-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『正式发行包、容器及三方依赖』下测得『SBOM齐全、构建物签名、授权许可证扫描完成』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 121 IT-NFR-ENV-001 — 支持环境-指标与质量门禁

- **对应要件：** NFR-ENV-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『Linux amd64/arm64、容器、K8s；Windows为CLI/客户端』下测得『支持矩阵中的环境可重复部署且升级回滚通过』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 122 IT-NFR-ENV-002 — 资源效率-指标与质量门禁

- **对应要件：** NFR-ENV-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『控制面中等负载（B1），监控处理器与内存』下测得『资源预算以性能基线测试建立，不以未验证的绝对值承诺』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 123 IT-NFR-ENV-003 — 离线可维护性-指标与质量门禁

- **对应要件：** NFR-ENV-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『企业内网环境』下测得『无公网时安装镜像、文档、许可证/密钥方案可正常运作』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 124 IT-NFR-ENV-004 — 地域节能策略-指标与质量门禁

- **对应要件：** NFR-ENV-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照
- **测试步骤：** 1) Linux amd64/arm64容器与K8s、Windows CLI、离线镜像仓库；配置快照；2) 断公网完成离线部署；分别验证amd64/arm64容器、K8s和Windows CLI的功能/升级；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『冷热层存储/生命周期需要遵守WORM及数据驻留』下测得『可呈现资源耗用及预估迁移成本，不因优化误删受保护版本』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 125 IT-ADV-S3-CLIENTS — 官方多语言SDK校验

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) aws-cli boto3 JS v3 Go Rust SDK，同签名/错误对照AWS；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** aws-cli boto3 JS v3 Go Rust SDK，同签名/错误对照AWS；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 126 IT-ADV-S3-CHECKSUM — MPU checksum header/trailer

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) CRC32C/sha256协商，未知算法拒绝，不默默跳过；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** CRC32C/sha256协商，未知算法拒绝，不默默跳过；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 127 IT-ADV-S3-XML — 错误XML和请求ID

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) NoSuchKey/BadDigest/AccessDenied/503 header/body完整；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** NoSuchKey/BadDigest/AccessDenied/503 header/body完整；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 128 IT-ADV-S3-PRESIGN — Gateway短URL撤销

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) revoked AccessKey即拒绝，provider直链不计承诺；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** revoked AccessKey即拒绝，provider直链不计承诺；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 129 IT-ADV-S3-HOST — TLS反向代理原始请求

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) Host/path签名前后不变，保留连续/和%2F；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** Host/path签名前后不变，保留连续/和%2F；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 130 IT-ADV-GIT-SKIPHOOK — 绕过客户端hook

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 启动真实GitGit Rust Core及MinIOS3；2) 创建新Manifest及DurableReceipt；3) 执行Commit Prepare/Ref CAS并注入错误/乱序/掉电；4) 核对Ref/AssetRevision/Manifest SHA/安全GC与审计Outbox；5) 若涉及LFS，请作为独立可选适配测试。
- **判定：** git -c core.hooksPath=/dev/null push仍遭GitServer阻断；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 131 IT-ADV-GIT-SIGNED — 伪造Webhook

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 启动真实GitGit Rust Core及MinIOS3；2) 创建新Manifest及DurableReceipt；3) 执行Commit Prepare/Ref CAS并注入错误/乱序/掉电；4) 核对Ref/AssetRevision/Manifest SHA/安全GC与审计Outbox；5) 若涉及LFS，请作为独立可选适配测试。
- **判定：** 签名错误/nonce重放拒绝，未授权不进入index；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 132 IT-ADV-GIT-LOCK — Lock verify ours/theirs

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 启动真实GitGit Rust Core及MinIOS3；2) 创建新Manifest及DurableReceipt；3) 执行Commit Prepare/Ref CAS并注入错误/乱序/掉电；4) 核对Ref/AssetRevision/Manifest SHA/安全GC与审计Outbox；5) 若涉及LFS，请作为独立可选适配测试。
- **判定：** 两个账号同一路径仅一人持锁，原Ref不更新；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 133 IT-ADV-GIT-REFGC — Tag+Fork引用GC

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 启动真实GitGit Rust Core及MinIOS3；2) 创建新Manifest及DurableReceipt；3) 执行Commit Prepare/Ref CAS并注入错误/乱序/掉电；4) 核对Ref/AssetRevision/Manifest SHA/安全GC与审计Outbox；5) 若涉及LFS，请作为独立可选适配测试。
- **判定：** 删除一branch但Tag+fork存在时不删除CAS；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 134 IT-ADV-MIG-LOCK — Federation缺Object Lock

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 能力Probe为Unsupported/Unverified时Cutover阻断；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 能力Probe为Unsupported/Unverified时Cutover阻断；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 135 IT-ADV-MIG-WATER — 乱序增量watermark

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 重放乱序delta不会逆向覆盖高Version；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 重放乱序delta不会逆向覆盖高Version；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 136 IT-ADV-MIG-REVERSE — Cutover后逆向追平

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) Target新ACK版被迁回Source前不能回旧主；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** Target新ACK版被迁回Source前不能回旧主；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 137 IT-ADV-META-DATA — fsync receipt+Raft提交序

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 故障P2/P3恢复对象要么未发布要么完整可读；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 故障P2/P3恢复对象要么未发布要么完整可读；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 138 IT-ADV-KMS-DENY — KMS失效与密钥轮换

- **对应要件：** FR-SEC-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 不明文回退，老对象所需KeyRef可追溯；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 不明文回退，老对象所需KeyRef可追溯；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 139 IT-ADV-AUDIT-SINK — Sink断网重复发送

- **对应要件：** FR-SEC-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) event ID去重且链式证据可重构；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** event ID去重且链式证据可重构；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 140 IT-ADV-MEDIA-SANDBOX — 解压炸弹/路径逃逸

- **对应要件：** FR-OPS-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 沙盒资源限制和出站网络禁止，原件不受影响；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 沙盒资源限制和出站网络禁止，原件不受影响；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 141 IT-FR-GG-001 — GitGit仓库权威 — IT

- **对应要件：** FR-GG-001 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“GitGit仓库权威”协议调用，核对返回结构与流式数据；3) 注入重复并发CAS、期待旧Ref修订号，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；停用所有外部Git兼容接口，仍能完成创建仓库、提交、分支与历史恢复。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 142 IT-FR-GG-002 — 不可变提交图 — IT

- **对应要件：** FR-GG-002 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“不可变提交图”协议调用，核对返回结构与流式数据；3) 注入主节点断电与ACK丢失，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；同输入稳定得到同Commit ID；不能通过正常API改写历史对象。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 143 IT-FR-GG-003 — 原子Ref CAS — IT

- **对应要件：** FR-GG-003 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“原子Ref CAS”协议调用，核对返回结构与流式数据；3) 注入重名路径和Unicode规范化，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；两名并发写者仅一个成功；失败者看见明确冲突且不产生部分Ref。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 144 IT-FR-GG-004 — 仓库快照 — IT

- **对应要件：** FR-GG-004 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“仓库快照”协议调用，核对返回结构与流式数据；3) 注入Outbox重复/乱序/永久缺失，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；重命名/删除之后checkout旧Commit按字节还原全部文件。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 145 IT-FR-GG-005 — 两阶段提交门禁 — IT

- **对应要件：** FR-GG-005 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“两阶段提交门禁”协议调用，核对返回结构与流式数据；3) 注入无权限读、票据过期与重试，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；缺片或Receipt过期时拒绝Ref发布，且保留可补偿事务。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 146 IT-FR-GG-006 — 幂等确认与Outbox — IT

- **对应要件：** FR-GG-006 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“幂等确认与Outbox”协议调用，核对返回结构与流式数据；3) 注入断网恢复时Ref已改变，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；断网+重放+乱序事件后两侧谱系最终对账一致，不产生重复版本。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 147 IT-FR-GG-007 — 树级变更与重命名 — IT

- **对应要件：** FR-GG-007 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“树级变更与重命名”协议调用，核对返回结构与流式数据；3) 注入Tag保护、ForcePush、旧Snapshot，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；重命名保留AssetId及全部历史；路径冲突不得静默覆盖。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 148 IT-FR-GG-008 — 分支差异查询 — IT

- **对应要件：** FR-GG-008 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“分支差异查询”协议调用，核对返回结构与流式数据；3) 注入仓库级ACL撤销与GC竞态，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；带1000个资产的样本差异结果可按AssetId/RevisionId复算。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 149 IT-FR-GG-009 — 原生资产锁 — IT

- **对应要件：** FR-GG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“原生资产锁”协议调用，核对返回结构与流式数据；3) 注入重复并发CAS、期待旧Ref修订号，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；并发锁冲突拒绝覆盖，强制解锁有审计并更新fencing token。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 150 IT-FR-GG-010 — 离线工作区日志 — IT

- **对应要件：** FR-GG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“离线工作区日志”协议调用，核对返回结构与流式数据；3) 注入主节点断电与ACK丢失，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；断网修改恢复后不误覆盖远端并提示冲突。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 151 IT-FR-GG-011 — 稀疏工作区与懒加载 — IT

- **对应要件：** FR-GG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“稀疏工作区与懒加载”协议调用，核对返回结构与流式数据；3) 注入重名路径和Unicode规范化，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；未选中的20GiB媒体不下载，选中内容下载SHA与Manifest一致。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 152 IT-FR-GG-012 — 三方合并引擎 — IT

- **对应要件：** FR-GG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“三方合并引擎”协议调用，核对返回结构与流式数据；3) 注入Outbox重复/乱序/永久缺失，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；不可安全合并时返回显式冲突，绝不自动选取后写者。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 153 IT-FR-GG-013 — 冲突决议审计 — IT

- **对应要件：** FR-GG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“冲突决议审计”协议调用，核对返回结构与流式数据；3) 注入无权限读、票据过期与重试，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；合并结果可复查全部parent和resolve原因。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 154 IT-FR-GG-014 — 媒体评审流 — IT

- **对应要件：** FR-GG-014 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体评审流”协议调用，核对返回结构与流式数据；3) 注入断网恢复时Ref已改变，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；未获必需审批无法发布指定受保护分支。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 155 IT-FR-GG-015 — 依赖变更影响 — IT

- **对应要件：** FR-GG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“依赖变更影响”协议调用，核对返回结构与流式数据；3) 注入Tag保护、ForcePush、旧Snapshot，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；修改纹理后定位全部引用该纹理的模型且不跨分支误报。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 156 IT-FR-GG-016 — 历史可达性水位 — IT

- **对应要件：** FR-GG-016 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“历史可达性水位”协议调用，核对返回结构与流式数据；3) 注入仓库级ACL撤销与GC竞态，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；丢事件或正在并发恢复时GC fail-closed且不得误删仍可达块。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 157 IT-FR-GG-017 — 组织级授权 — IT

- **对应要件：** FR-GG-017 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“组织级授权”协议调用，核对返回结构与流式数据；3) 注入重复并发CAS、期待旧Ref修订号，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；撤权与过期读票据不会绕开资源级ACL。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 158 IT-FR-GG-018 — 受保护提交策略 — IT

- **对应要件：** FR-GG-018 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“受保护提交策略”协议调用，核对返回结构与流式数据；3) 注入主节点断电与ACK丢失，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；无权force-update拒绝且所有策略决策留痕。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 159 IT-FR-GG-019 — 标准Git迁入隔离 — IT

- **对应要件：** FR-GG-019 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“标准Git迁入隔离”协议调用，核对返回结构与流式数据；3) 注入重名路径和Unicode规范化，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；禁用互操作插件后原生核心可运行；迁移映射差异清晰可查。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 160 IT-FR-GG-020 — 提交与资产恢复 — IT

- **对应要件：** FR-GG-020 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“提交与资产恢复”协议调用，核对返回结构与流式数据；3) 注入Outbox重复/乱序/永久缺失，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；模拟跨存储迁移后历史恢复字节正确。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 161 IT-FR-MV-001 — 稳定资产标识 — IT

- **对应要件：** FR-MV-001 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“稳定资产标识”协议调用，核对返回结构与流式数据；3) 注入Chunk大小边界与文件前段插入，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；移动/改名不改变AssetId，更新内容必生成新Revision。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 162 IT-FR-MV-002 — 媒体版本记录 — IT

- **对应要件：** FR-MV-002 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体版本记录”协议调用，核对返回结构与流式数据；3) 注入篡改Chunk/Manifest与重试校验，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；同一Asset存在分叉与合并时可画出完整版本DAG。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 163 IT-FR-MV-003 — 内容清单 — IT

- **对应要件：** FR-MV-003 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“内容清单”协议调用，核对返回结构与流式数据；3) 注入跨租户去重存在性推断，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；任意读取块可按清单重组并与原始SHA-256完全一致。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 164 IT-FR-MV-004 — 内容定义分块 — IT

- **对应要件：** FR-MV-004 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“内容定义分块”协议调用，核对返回结构与流式数据；3) 注入无法解析媒体并触发隔离限额，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；前部插入小内容后后续大部分相同Chunk能够复用，参数与分块版本固定。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 165 IT-FR-MV-005 — 强摘要验证 — IT

- **对应要件：** FR-MV-005 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“强摘要验证”协议调用，核对返回结构与流式数据；3) 注入Adapter升级、能力降级与许可证失败，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；随机篡改任意Chunk或Manifest必被拒绝/隔离，不能返回成功ACK。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 166 IT-FR-MV-006 — 安全租户去重 — IT

- **对应要件：** FR-MV-006 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“安全租户去重”协议调用，核对返回结构与流式数据；3) 注入三方merge冲突与并行修改，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；Tenant B测时/响应无法判定Tenant A内容是否存在，B无权取A的Chunk。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 167 IT-FR-MV-007 — 断点续传 — IT

- **对应要件：** FR-MV-007 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“断点续传”协议调用，核对返回结构与流式数据；3) 注入S3物理版本变化及迁移Cutover，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；20GiB模拟流断线后仅补传缺块，不重复生成同一可见Revision。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 168 IT-FR-MV-008 — 增量下载 — IT

- **对应要件：** FR-MV-008 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“增量下载”协议调用，核对返回结构与流式数据；3) 注入GC/回滚和已ACK资产的并发可达性，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；部分请求字节/offset正确，内存占用不随文件总大小线性增长。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 169 IT-FR-MV-009 — 原件不可变 — IT

- **对应要件：** FR-MV-009 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“原件不可变”协议调用，核对返回结构与流式数据；3) 注入Chunk大小边界与文件前段插入，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；所有派生对象sha不同空间，重取旧Commit的原件字节完全一致。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 170 IT-FR-MV-010 — 媒体探测 — IT

- **对应要件：** FR-MV-010 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体探测”协议调用，核对返回结构与流式数据；3) 注入篡改Chunk/Manifest与重试校验，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；伪造扩展名不能自动触发高风险解析器，未知类型安全保存。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 171 IT-FR-MV-011 — 可扩展元数据 — IT

- **对应要件：** FR-MV-011 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“可扩展元数据”协议调用，核对返回结构与流式数据；3) 注入跨租户去重存在性推断，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；未知字段可往返且Schema升级不破坏原始数据。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 172 IT-FR-MV-012 — 媒体适配器统一接口 — IT

- **对应要件：** FR-MV-012 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体适配器统一接口”协议调用，核对返回结构与流式数据；3) 注入无法解析媒体并触发隔离限额，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；不支持能力返回Unsupported而非默认成功，Adapter升级不修改旧Revision。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 173 IT-FR-MV-013 — 图像版本差异 — IT

- **对应要件：** FR-MV-013 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“图像版本差异”协议调用，核对返回结构与流式数据；3) 注入Adapter升级、能力降级与许可证失败，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；参考图golden数据可复算热区，PSDs不支持图层时降级为渲染图比较。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 174 IT-FR-MV-014 — 视频时间线差异 — IT

- **对应要件：** FR-MV-014 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“视频时间线差异”协议调用，核对返回结构与流式数据；3) 注入三方merge冲突与并行修改，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；变更片段时间码与样本golden一致，不能把非确定性转码当原内容证据。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 175 IT-FR-MV-015 — 音频版本差异 — IT

- **对应要件：** FR-MV-015 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“音频版本差异”协议调用，核对返回结构与流式数据；3) 注入S3物理版本变化及迁移Cutover，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；参考音频能定位差异区间，未标准化采样率须显式提示。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 176 IT-FR-MV-016 — 三维资产差异 — IT

- **对应要件：** FR-MV-016 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“三维资产差异”协议调用，核对返回结构与流式数据；3) 注入GC/回滚和已ACK资产的并发可达性，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；相同拓扑不同材质及相同外观不同网格被正确区别。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 177 IT-FR-MV-017 — 安全合并能力声明 — IT

- **对应要件：** FR-MV-017 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“安全合并能力声明”协议调用，核对返回结构与流式数据；3) 注入Chunk大小边界与文件前段插入，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；两个不可合并二进制冲突绝不自动丢失一方修改。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 178 IT-FR-MV-018 — 资源依赖DAG — IT

- **对应要件：** FR-MV-018 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“资源依赖DAG”协议调用，核对返回结构与流式数据；3) 注入篡改Chunk/Manifest与重试校验，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；循环引用检测、跨Commit依赖变更反向追踪正确。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 179 IT-FR-MV-019 — 多文件原子资源快照 — IT

- **对应要件：** FR-MV-019 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“多文件原子资源快照”协议调用，核对返回结构与流式数据；3) 注入跨租户去重存在性推断，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；断电时不会出现Commit A包含一半新素材一半未确认素材。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 180 IT-FR-MV-020 — 媒体派生缓存隔离 — IT

- **对应要件：** FR-MV-020 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体派生缓存隔离”协议调用，核对返回结构与流式数据；3) 注入无法解析媒体并触发隔离限额，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；缓存污染或过期不影响原始数据，可清除并重建。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 181 IT-FR-MV-021 — 隔离媒体解析 — IT

- **对应要件：** FR-MV-021 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“隔离媒体解析”协议调用，核对返回结构与流式数据；3) 注入Adapter升级、能力降级与许可证失败，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；构造ZIP炸弹/格式畸形样本，Worker触发限额时主服务不崩溃。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 182 IT-FR-MV-022 — 媒体权限和分享 — IT

- **对应要件：** FR-MV-022 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体权限和分享”协议调用，核对返回结构与流式数据；3) 注入三方merge冲突与并行修改，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；撤权后旧预览URL及Range不能继续访问受保护源。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 183 IT-FR-MV-023 — 资产搜索索引 — IT

- **对应要件：** FR-MV-023 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“资产搜索索引”协议调用，核对返回结构与流式数据；3) 注入S3物理版本变化及迁移Cutover，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；权限过滤正确且展示index watermark，允许索引延迟但不伪造提交状态。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 184 IT-FR-MV-024 — 冲突可视化 — IT

- **对应要件：** FR-MV-024 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“冲突可视化”协议调用，核对返回结构与流式数据；3) 注入GC/回滚和已ACK资产的并发可达性，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；不支持内容显示元数据差异、哈希和人工冲突入口。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 185 IT-FR-MV-025 — 作品来源与授权 — IT

- **对应要件：** FR-MV-025 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“作品来源与授权”协议调用，核对返回结构与流式数据；3) 注入Chunk大小边界与文件前段插入，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；授权过期的外部媒体阻断发布，下载与审计保留可授权访问。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 186 IT-FR-MV-026 — 可验证媒体导出 — IT

- **对应要件：** FR-MV-026 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“可验证媒体导出”协议调用，核对返回结构与流式数据；3) 注入篡改Chunk/Manifest与重试校验，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；导出的包在离线验证工具中重建SHA与版本谱系一致。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 187 IT-FR-MV-027 — 存储逻辑与业务版本隔离 — IT

- **对应要件：** FR-MV-027 | 阶段 E0 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“存储逻辑与业务版本隔离”协议调用，核对返回结构与流式数据；3) 注入跨租户去重存在性推断，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；迁移后GitGit revision不改变，即使目标Provider versionId完全不同。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 188 IT-FR-MV-028 — 安全垃圾回收 — IT

- **对应要件：** FR-MV-028 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“安全垃圾回收”协议调用，核对返回结构与流式数据；3) 注入无法解析媒体并触发隔离限额，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；在并发checkout/restore与GC竞态中绝不删仍有引用的Chunk。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 189 IT-FR-MV-029 — 块级迁移 — IT

- **对应要件：** FR-MV-029 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“块级迁移”协议调用，核对返回结构与流式数据；3) 注入Adapter升级、能力降级与许可证失败，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；切换期间所有受保留Commit可恢复，迁移后块SHA不变。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 190 IT-FR-MV-030 — 媒体协作事件 — IT

- **对应要件：** FR-MV-030 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“媒体协作事件”协议调用，核对返回结构与流式数据；3) 注入三方merge冲突与并行修改，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；单资产事件乱序处理后最终timeline与权威Commit序列一致。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 191 IT-FR-MV-031 — 批量导入与目录映射 — IT

- **对应要件：** FR-MV-031 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“批量导入与目录映射”协议调用，核对返回结构与流式数据；3) 注入S3物理版本变化及迁移Cutover，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；导入重试不会重复资产；目录相对路径和大小写冲突按策略报告。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 192 IT-FR-MV-032 — 面向生产的预览代理 — IT

- **对应要件：** FR-MV-032 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“面向生产的预览代理”协议调用，核对返回结构与流式数据；3) 注入GC/回滚和已ACK资产的并发可达性，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；代理生成失败不阻止GitGit Checkout；展示派生版本与处理参数。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 193 IT-FR-MV-033 — 复杂工程资源类型 — IT

- **对应要件：** FR-MV-033 | 阶段 E2 | 优先级 SHOULD
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“复杂工程资源类型”协议调用，核对返回结构与流式数据；3) 注入Chunk大小边界与文件前段插入，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；缺少某Adapter不影响完整二进制保存，依赖分析显式标Unsupported。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 194 IT-FR-MV-034 — 资产语义标签 — IT

- **对应要件：** FR-MV-034 | 阶段 E1 | 优先级 MUST
- **前提：** 启动真实GitGit Rust Core + MinIOS3 Gateway/Meta；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 在隔离Repo/Temp Bucket启动真实GitGit/MinIOS3接口并记录版本；2) 发送“资产语义标签”协议调用，核对返回结构与流式数据；3) 注入篡改Chunk/Manifest与重试校验，在调用中断/重试后读取GitGit Ref、Manifest和Chunk；4) 交叉核对服务审计、watermark、sha256、状态机checkpoint。
- **判定：** 两个服务的确认边界和身份映射一致，错误未产生半提交或越权；可查询某Commit下标签快照，后改标签不篡改提交历史。
- **证据：** HTTP/gRPC trace(redacted) + GitGit RefTxn + Manifest/Chunk SHA + service logs
- **方法：** CI integration disposable cluster
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可
