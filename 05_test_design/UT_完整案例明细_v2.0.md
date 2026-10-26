# UT 完整测试案例明细 — GitGit + MinIOS3 v2.0

> 共 186条设计案例 | Not Run | 所有Case ID唯一；旧案例沿用编号，新业务主链路按原生GitGit验收。

## 001 UT-FR-CORE-001 — 独立部署-纯逻辑与错误边界

- **对应要件：** FR-CORE-001 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 验证新要求：断开公网后，GitGit原生提交、版本回退、媒体分块上传/下载、MinIOS3 PUT/GET/LIST和管理可正常完成；禁用LFS适配仍通过。；关闭Git LFS兼容适配时原生媒体版本提交仍必须可用。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 002 UT-FR-CORE-002 — Native 与 Federation 运行模式-纯逻辑与错误边界

- **对应要件：** FR-CORE-002 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：两种部署模式均通过 S3 核心兼容测试；模式切换不得改变现有对象内容。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 003 UT-FR-CORE-003 — 租户层级-纯逻辑与错误边界

- **对应要件：** FR-CORE-003 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：租户 A 凭据无法枚举或读取租户 B 的任何资源；隔离自动测试100%通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 004 UT-FR-CORE-004 — 租户资源计量和配额-纯逻辑与错误边界

- **对应要件：** FR-CORE-004 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：超出硬配额返回明确错误，管理员可追踪来源；计量误差可核对。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 005 UT-FR-CORE-005 — 存储命名空间和资源映射-纯逻辑与错误边界

- **对应要件：** FR-CORE-005 | 阶段 E0 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：迁移后使用原端点、原 Bucket/Key 可以正确读取原版本。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 006 UT-FR-CORE-006 — 配置版本与回滚-纯逻辑与错误边界

- **对应要件：** FR-CORE-006 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：错误路由配置可恢复上一生效版本，不改变已确认对象。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 007 UT-FR-CORE-007 — 多区域和数据驻留策略-纯逻辑与错误边界

- **对应要件：** FR-CORE-007 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：禁止区域间转移时，对应迁移/复制任务无法启动并写入审计记录。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 008 UT-FR-CORE-008 — 能力发现接口-纯逻辑与错误边界

- **对应要件：** FR-CORE-008 | 阶段 E1 | 优先级 MUST
- **前提：** 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体
- **测试步骤：** 1) 固定Tenant-A/Tenant-B、项目/桶/配额、route epoch 和配置revision；构造授权及跨租户主体；2) Mock GlobalCatalog/ConfigStore/PolicyEngine，在内存状态机执行一次命令，再改变tenant、epoch、quota或revision重复；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 状态机不跨Tenant改变资源；冲突/旧epoch失败且无部分副作用；逐项验收：API和UI可取得对应版本的能力清单；不支持功能明确返回非兼容错误。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 009 UT-FR-S3-001 — Bucket 生命周期-纯逻辑与错误边界

- **对应要件：** FR-S3-001 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：创建、列举、空桶删除、权限错误及地域配置测试通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 010 UT-FR-S3-002 — 标准对象读写-纯逻辑与错误边界

- **对应要件：** FR-S3-002 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：主流 S3 SDK 的上传、下载、覆盖、条件请求符合兼容基线。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 011 UT-FR-S3-003 — 对象列举-纯逻辑与错误边界

- **对应要件：** FR-S3-003 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 预置 Key [a,a/1,a//b,汉字,emoji], prefix/delimiter筛选；同时插入100新key并用同一快照token翻页；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 每页最多max-keys，字节排序无重复无遗漏；篡改token/跨tenant token拒绝；对照原要件验收：千万级键空间分页不漏不重；与提交确认的写入符合指定一致性。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** REV-010:跨range强LIST未证明前只测单Bucket单Group

## 012 UT-FR-S3-004 — 分段上传-纯逻辑与错误边界

- **对应要件：** FR-S3-004 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) CreateMultipart；乱序上传Part 3/1/2；并发Complete与Abort；对已提交session重试并断开HTTP；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 只有一次ObjectVersion publish，parts排序准确；未发布part对GET不可见；对照原要件验收：分段乱序、断线续传、重复Complete、Abort及并发重试通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 013 UT-FR-S3-005 — 版本控制-纯逻辑与错误边界

- **对应要件：** FR-S3-005 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 在NeverEnabled→Enabled→Suspended→Enabled下依次PUT/DELETE含显式VersionId、null及DeleteMarker；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** VersionId稳定，旧版合法GET，锁定版本不可删除，DeleteMarker符合兼容矩阵；对照原要件验收：覆盖与删除后，历史版本可依授权恢复，版本标识稳定且唯一。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 014 UT-FR-S3-006 — 签名鉴权-纯逻辑与错误边界

- **对应要件：** FR-S3-006 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 准备已知AWS SigV4 canonical向量，依次修改Host、%2F、SignedHeaders、X-Amz-Date、payload bytes；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 签名任何参与字段被改即403，TLS代理未修改原始Host/Path时合法签名成功；对照原要件验收：官方AWS SDK认证场景及坏签名、过期时间戳均有正确响应。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 015 UT-FR-S3-007 — 临时预签名-纯逻辑与错误边界

- **对应要件：** FR-S3-007 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：过期、越权、撤销后无法访问；范围约束与审计关联正确。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 016 UT-FR-S3-008 — HTTP Range 与流式传输-纯逻辑与错误边界

- **对应要件：** FR-S3-008 | 阶段 E0 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：对≥20GiB对象进行Range读取，不整体加载内存，返回正确206与校验。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 017 UT-FR-S3-009 — 对象属性及标签-纯逻辑与错误边界

- **对应要件：** FR-S3-009 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：元数据/标签在复制迁移后按策略完整保留。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 018 UT-FR-S3-010 — 分层策略-纯逻辑与错误边界

- **对应要件：** FR-S3-010 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：默认拒绝、多租户隔离、策略冲突和SDK常见操作矩阵通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 019 UT-FR-S3-011 — 生命周期管理-纯逻辑与错误边界

- **对应要件：** FR-S3-011 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：TTL到期任务正确执行；被保留/保护的版本不被误删。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 020 UT-FR-S3-012 — Object Lock-纯逻辑与错误边界

- **对应要件：** FR-S3-012 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 旧Version设Governance和Compliance，试Governance绕过、有无权限、缩短Compliance、LegalHold解除、时钟回拨；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Compliance始终拒绝提前物理删除；新Version/合法DeleteMarker按协议允许；对照原要件验收：WORM对覆盖、版本删除、撤销保留、时钟回拨等攻击测试拒绝；独立审计完成。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 021 UT-FR-S3-013 — 事件通知-纯逻辑与错误边界

- **对应要件：** FR-S3-013 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：消费者重复收到事件也能幂等处理；失败可重放。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 022 UT-FR-S3-014 — SSE/KMS兼容-纯逻辑与错误边界

- **对应要件：** FR-S3-014 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：使用配置KMS读写正常，密钥拒绝/轮换行为受测。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 023 UT-FR-S3-015 — Object Checksum-纯逻辑与错误边界

- **对应要件：** FR-S3-015 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：篡改块校验失败；迁移前后SHA-256一致。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 024 UT-FR-S3-016 — S3 Batch 操作-纯逻辑与错误边界

- **对应要件：** FR-S3-016 | 阶段 E2 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：10万对象批量任务中断重启不丢状态、无重复破坏性变更。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 025 UT-FR-S3-017 — S3 兼容性清单-纯逻辑与错误边界

- **对应要件：** FR-S3-017 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：发布版本附机器可读兼容清单，并对宣称支持的API自动回归。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 026 UT-FR-S3-018 — 虚拟主机与路径风格-纯逻辑与错误边界

- **对应要件：** FR-S3-018 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：两种地址形式在合法桶名、特殊字符、代理TLS下通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 027 UT-FR-S3-019 — 对象幂等与并发条件-纯逻辑与错误边界

- **对应要件：** FR-S3-019 | 阶段 E1 | 优先级 MUST
- **前提：** 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture
- **测试步骤：** 1) 创建S3 Bucket-A，准备Key含空格、%2F、连续斜杠、UTF-8；含版本启停、Tag、MPU、签名fixture；2) 使用固定Clock、RawRequest/SigV4 fixture、Catalog与Backend mock，对同一API执行合法与边界输入；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 输出Code/Header/XML一致；对象版本单次提交，失败不能出现可见半对象；逐项验收：重试不会生成损坏版本；并发相同键操作呈现可解释顺序。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 028 UT-FR-GIT-001 — [v2修订] Git LFS Batch API - UT验证

- **对应要件：** FR-GIT-001 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：LFS兼容客户端传输与SHA校验通过；禁用LFS兼容服务不影响GitGit原生E0媒体版本链路。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 029 UT-FR-GIT-002 — [v2修订] Git LFS Locking API - UT验证

- **对应要件：** FR-GIT-002 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：GitGit锁与LFS锁查询一致；未启用LFS时原生协作锁仍有效。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 030 UT-FR-GIT-003 — [v2修订] 标准Git独立性 - UT验证

- **对应要件：** FR-GIT-003 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：关闭标准Git互操作插件后GitGit原生版本管理仍能提交、分支、比较和恢复。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 031 UT-FR-GIT-004 — [v2修订] 内容寻址 - UT验证

- **对应要件：** FR-GIT-004 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：同字节同策略域复用且不改变原件；租户隔离，读取重新校验。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 032 UT-FR-GIT-005 — [v2修订] 引用完整性检查 - UT验证

- **对应要件：** FR-GIT-005 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：缺块、旧epoch、非法ACL或过期票据均拒绝提交；旧Ref保持不变。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 033 UT-FR-GIT-006 — [v2修订] 版本关联索引 - UT验证

- **对应要件：** FR-GIT-006 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：Commit→资产→内容块→物理存放可逆向追踪且hash验证一致。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 034 UT-FR-GIT-007 — [v2修订] Git分支/Tag与媒体历史 - UT验证

- **对应要件：** FR-GIT-007 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：创建、切换、比较、回退GitGit分支正确还原媒体内容；S3版本ID差异不影响历史。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 035 UT-FR-GIT-008 — [v2修订] Git身份继承 - UT验证

- **对应要件：** FR-GIT-008 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：撤销GitGit权限后新的原生读取及签名URL请求被拒；跨租户不可枚举CAS。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 036 UT-FR-GIT-009 — [v2修订] Git事件接入 - UT验证

- **对应要件：** FR-GIT-009 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：重复、乱序、遗漏后通过快照watermark对账；GC对未证完整可达性拒删。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 037 UT-FR-GIT-010 — [v2修订] 媒体垃圾回收安全 - UT验证

- **对应要件：** FR-GIT-010 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：在并发新引用、迁移、快照与合法保留期间零误删。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 038 UT-FR-GIT-011 — [v2修订] 离线和部分克隆 - UT验证

- **对应要件：** FR-GIT-011 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：离线提交后重连按冲突策略重新校验；只下载请求Chunk并保持SHA一致。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 039 UT-FR-GIT-012 — 仓库配额和限流-纯逻辑与错误边界

- **对应要件：** FR-GIT-012 | 阶段 E1 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff；2) Mock可信Git Snapshot、RepoGrant、OID物理receipt、Lock目录；执行授权/prepare/GC判定并重放event；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 同OID字节不被替换；无RepoGrant不可访问；签名/水位/锁失败时fail-closed；逐项验收：超配额上传在写入前可预测拒绝，已存在历史不被误删。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 040 UT-FR-GIT-013 — [v2修订] 提交一致性补偿 - UT验证

- **对应要件：** FR-GIT-013 | 阶段 E0 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：断电、超时、重复请求不会产生可见缺块提交；补偿可恢复且GC保护。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 041 UT-FR-GIT-014 — [v2修订] 现有Git迁移 - UT验证

- **对应要件：** FR-GIT-014 | 阶段 E2 | 优先级 MUST
- **前提：** 初始化Repo-A/Repo-B、受信Git Server stub、两组Commit/Tag/分支与含LFS pointer的tree diff
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：导入后保留历史关系与内容摘要且输出无法映射特性的差异报告。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 042 UT-FR-MEDIA-001 — 多媒体资产分类-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-001 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：至少PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP完成端到端验证。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 043 UT-FR-MEDIA-002 — 资产元数据-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-002 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：元数据与Git Commit及OID可以双向检索。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 044 UT-FR-MEDIA-003 — 内容零改写-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-003 | 阶段 E0 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：下载SHA-256与原上传文件相同。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 045 UT-FR-MEDIA-004 — 异步预览-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：预览出错时下载与版本读取不受影响，且有失败原因。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 046 UT-FR-MEDIA-005 — [v2修订] 版本差异辅助 - UT验证

- **对应要件：** FR-MEDIA-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 建立GitGit原生Repo和可信媒体Manifest；2) 按原接口向GitGit/MinIOS3提交或读取；3) 注入错误epoch/未持久化Chunk/并发Ref CAS等负例；4) 如果该要求属于Git LFS兼容则只使用可选Adapter并单独报告；5) 核对版本、授权、Hash、审计与设计接受条件。
- **判定：** 按v2.0新版要求验证：通过媒体专用golden fixture识别变化，无法识别时回退hash/size而不宣称语义相等。；GitGit原生版本通道不得依赖旧LFS兼容路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 如GitGit原生核心未实现，该需求按其发布阶段标记Blocked；LFS兼容不等于原生通过。

## 047 UT-FR-MEDIA-006 — 资产搜索-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-006 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：指定索引规模下按分页检索，结果遵循权限隔离。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 048 UT-FR-MEDIA-007 — 媒体预览安全-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：构造异常媒体文件不能访问宿主机密钥或其他租户对象。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 049 UT-FR-MEDIA-008 — 预览内容与源关联-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-008 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：源变化触发可识别的重新生成；源内容保持不可变。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 050 UT-FR-MEDIA-009 — 媒体交付-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：断网后续传与视频Seek测试通过；缓存不泄漏租户数据。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 051 UT-FR-MEDIA-010 — 许可与溯源-纯逻辑与错误边界

- **对应要件：** FR-MEDIA-010 | 阶段 E2 | 优先级 MUST
- **前提：** 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本
- **测试步骤：** 1) 准备含PNG/JPEG/TIFF/MP4/MOV/WAV/FLAC/GLB/FBX/PSD/ZIP及异常头部的SHA-256样本；2) 用固定sample bytes和恶意样本触发MIME鉴别、SourceOID关联与Preview state转换；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 源文件SHA不变；预览失败只修改派生状态且无未授权访问；逐项验收：受限制素材无法被标记为可公开发布。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 052 UT-FR-XCHG-001 — S3后端适配-纯逻辑与错误边界

- **对应要件：** FR-XCHG-001 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：同一测试数据在至少AWS S3与另一个自建S3实现上完整往返。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 053 UT-FR-XCHG-002 — 非S3供应商适配-纯逻辑与错误边界

- **对应要件：** FR-XCHG-002 | 阶段 E2 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：增补适配器无需更改S3客户端公开接口。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 054 UT-FR-XCHG-003 — 统一端点-纯逻辑与错误边界

- **对应要件：** FR-XCHG-003 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：原有客户端仅改Endpoint/密钥即可访问受支持S3 API。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 055 UT-FR-XCHG-004 — 后端能力矩阵-纯逻辑与错误边界

- **对应要件：** FR-XCHG-004 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：要求WORM而后端不满足时禁止部署，不允许虚假成功。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 056 UT-FR-XCHG-005 — 存储策略与放置-纯逻辑与错误边界

- **对应要件：** FR-XCHG-005 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：Dry-run输出每条规则的匹配原因和成本估计。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 057 UT-FR-XCHG-006 — 迁移作业-纯逻辑与错误边界

- **对应要件：** FR-XCHG-006 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 两个Provider对同一版本赋予不同physical VersionId，抽取旧版/delete marker/metadata/Tag/Lock后杀死Worker；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** Checkpoint重放不漏Version；public逻辑VersionId保持，源/目标SHA及length一致；对照原要件验收：百万对象迁移中断后恢复，不漏文件；核验摘要与元数据。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 058 UT-FR-XCHG-007 — 迁移预演-纯逻辑与错误边界

- **对应要件：** FR-XCHG-007 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：存在不兼容Object Lock时必须报阻断项。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 059 UT-FR-XCHG-008 — 无损切换-纯逻辑与错误边界

- **对应要件：** FR-XCHG-008 | 阶段 E0 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 完成Cutover后在新主写入V2并收到ACK，随后模拟目标故障请求立即回旧源；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 旧源未追平时必须阻断/只读；反向追平验证后才能CAS切回，V2仍可GET；对照原要件验收：生产演练迁移失败可回退到源端，用户已确认写入不丢失。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 060 UT-FR-XCHG-009 — 双端校验-纯逻辑与错误边界

- **对应要件：** FR-XCHG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) 人为损坏目标某个历史Version和Tag，执行完整Verification；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 分别输出HashMismatch和MetadataMismatch，任何差异阻断Cutover；对照原要件验收：对人为损坏和缺失分别准确告警。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 061 UT-FR-XCHG-010 — 连续复制-纯逻辑与错误边界

- **对应要件：** FR-XCHG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：乱序和网络分区不产生静默覆盖；复制滞后指标可见。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 062 UT-FR-XCHG-011 — 冲突控制-纯逻辑与错误边界

- **对应要件：** FR-XCHG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：并发写同键情况下不允许最后写入静默掩盖冲突。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 063 UT-FR-XCHG-012 — 读取故障转移-纯逻辑与错误边界

- **对应要件：** FR-XCHG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：故障转移返回一致性状态；未同步数据不得误报存在。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 064 UT-FR-XCHG-013 — 后端出口开放-纯逻辑与错误边界

- **对应要件：** FR-XCHG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：第三方工具可从导出的数据重建完整键空间及版本关系。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 065 UT-FR-XCHG-014 — 成本与计量-纯逻辑与错误边界

- **对应要件：** FR-XCHG-014 | 阶段 E2 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：成本报表能与供应商账单进行差额核对。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 066 UT-FR-XCHG-015 — 底层供应商异常隔离-纯逻辑与错误边界

- **对应要件：** FR-XCHG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端
- **测试步骤：** 1) 注册AWS S3和自建兼容S3的专用测试桶，并配置物理VersionId不同、WORM能力不同的两份后端；2) Mock source/target Capability、不同PhysicalVersionId、路由Epoch、重复或乱序outbox事件；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 无法等价保留历史/锁即Blocked；Route CAS单主写；重放幂等；逐项验收：一个后端故障不会拖垮其它桶的正常请求。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 067 UT-FR-NATIVE-001 — 多节点横向扩展-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-001 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：扩容演练不需要停掉全部S3服务。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 068 UT-FR-NATIVE-002 — 纠删码/数据保护-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-002 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 依次验证6/6、5/6、4/6 durable receipt；在PUT步骤P2/P3/P4断电/断网；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 仅6片达标且Raft Commit后可ACK；其他进入orphan/unknown结果并可补偿；对照原要件验收：配置容错范围内故障可读取完整数据，超出范围进入安全只读/错误状态。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 069 UT-FR-NATIVE-003 — 故障域意识-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-003 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：放置审计证明碎片满足故障域策略。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 070 UT-FR-NATIVE-004 — 节点自愈与巡检-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-004 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：混合故障注入后数据摘要不变，进度和剩余风险可查询。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 071 UT-FR-NATIVE-005 — 存储一致性语义-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-005 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 对同Key并发覆盖/删/GET/HEAD/LIST并在leader切换和快照压缩时执行；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ACK后读一致、无半对象；单Bucket单Group，跨range模式未证明前disabled；对照原要件验收：ACK后GET/HEAD/LIST（在规定作用域）立即可见，不出现半对象。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** REV-010:跨range强LIST未证明前只测单Bucket单Group

## 072 UT-FR-NATIVE-006 — 数据与控制面故障隔离-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-006 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：单点注入与恢复试验通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 073 UT-FR-NATIVE-007 — 容量水位保护-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-007 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：容量接近上限能预测并安全拒绝写入。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 074 UT-FR-NATIVE-008 — 数据损坏检测-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-008 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：注入存储位错误后检测到并在冗余足够时自愈。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 075 UT-FR-NATIVE-009 — 故障演练接口-纯逻辑与错误边界

- **对应要件：** FR-NATIVE-009 | 阶段 E1 | 优先级 MUST
- **前提：** 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转
- **测试步骤：** 1) 用6节点3故障域4+2 EC fixture，Raft 3 voters，模拟磁盘故障、延迟fsync和位翻转；2) 用确定性FakeDisk、EC K4M2、Meta Mock验证Placement/receipt/missing shard和scrub；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不满6片不ACK；错误分片隔离，恢复摘要与输入一致；逐项验收：每个发布候选版本执行故障注入测试。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 076 UT-FR-SEC-001 — SSO与身份联邦-纯逻辑与错误边界

- **对应要件：** FR-SEC-001 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：SSO登录/登出/禁用账号后会话生效性经测试。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 077 UT-FR-SEC-002 — IAM/RBAC/ABAC-纯逻辑与错误边界

- **对应要件：** FR-SEC-002 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：尝试越权访问、横向提权和策略覆盖均失败。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 078 UT-FR-SEC-003 — 短期凭据-纯逻辑与错误边界

- **对应要件：** FR-SEC-003 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：过期Token和已撤销AccessKey被拒绝并审计。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 079 UT-FR-SEC-004 — 传输加密-纯逻辑与错误边界

- **对应要件：** FR-SEC-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：未加密入口默认关闭；TLS配置可扫描验证。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 080 UT-FR-SEC-005 — 静态加密与密钥管理-纯逻辑与错误边界

- **对应要件：** FR-SEC-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 在KMS revoke/轮换/timeout期间上传读历史Version，变更DEK wrapper；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 不明文回退，旧版授权读仍可验证或明确拒绝；审计完整；对照原要件验收：KMS故障不会造成明文回退，轮换后的旧数据可访问。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 081 UT-FR-SEC-006 — 审计追踪-纯逻辑与错误边界

- **对应要件：** FR-SEC-006 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：追踪一个Git Commit至对应OID和S3 Version与访问日志。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 082 UT-FR-SEC-007 — 不可篡改审计-纯逻辑与错误边界

- **对应要件：** FR-SEC-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 阻断外部审计Sink 30min、重启outbox sender、篡改Sink中历史记录；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 日志队列不丢且事件去重；篡改hash链告警；普通Ops无法重写；对照原要件验收：系统管理员不能通过普通运维权限覆盖已封存审计记录。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 083 UT-FR-SEC-008 — 恶意软件与内容控制-纯逻辑与错误边界

- **对应要件：** FR-SEC-008 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：恶意媒体可隔离并可追查，原始SHA-256和隔离动作有证据。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 084 UT-FR-SEC-009 — 管理面防护-纯逻辑与错误边界

- **对应要件：** FR-SEC-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：删除存储池、强制解锁、迁移切换触发审批规则。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 085 UT-FR-SEC-010 — 安全加固与供应链-纯逻辑与错误边界

- **对应要件：** FR-SEC-010 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：关键漏洞阻止正式发布，SBOM可下载。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 086 UT-FR-SEC-011 — 合法持有与数据隔离-纯逻辑与错误边界

- **对应要件：** FR-SEC-011 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：租户删除前检测WORM和Git活跃引用，依法合规执行。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 087 UT-FR-SEC-012 — 秘密管理-纯逻辑与错误边界

- **对应要件：** FR-SEC-012 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：数据导出和日志不包含明文AccessKey/SecretKey。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 088 UT-FR-SEC-013 — 勒索防护-纯逻辑与错误边界

- **对应要件：** FR-SEC-013 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：批量误删后可以按保留策略恢复；模拟攻击产生报警。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 089 UT-FR-SEC-014 — 合规声明边界-纯逻辑与错误边界

- **对应要件：** FR-SEC-014 | 阶段 E1 | 优先级 MUST
- **前提：** 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink
- **测试步骤：** 1) 准备OIDC/STS凭据、显式Deny/IAM Policy、测试KMS、Governance/Compliance/LegalHold以及外部审计sink；2) 用PolicyEngine、FakeClock、KMS mock、锁定版本状态和审计outbox执行允许/拒绝路径；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** ExplicitDeny和Compliance优先，KMS拒绝不降级，审计可追溯；逐项验收：发布材料包含适用范围、例外、证据和未认证声明。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 090 UT-FR-OPS-001 — Web管理控制台-纯逻辑与错误边界

- **对应要件：** FR-OPS-001 | 阶段 E0 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：任务列表和对象详情三步内可进入，角色不可见不属于自己的资源。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 091 UT-FR-OPS-002 — 版本关系可视化-纯逻辑与错误边界

- **对应要件：** FR-OPS-002 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：定位一项历史资源无需理解后端私有路径。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 092 UT-FR-OPS-003 — 迁移与复制工作台-纯逻辑与错误边界

- **对应要件：** FR-OPS-003 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：操作可审计；危险动作有影响预览。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 093 UT-FR-OPS-004 — 媒体浏览器-纯逻辑与错误边界

- **对应要件：** FR-OPS-004 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：图像、视频、3D预览异常不阻断下载。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 094 UT-FR-OPS-005 — 多语言-纯逻辑与错误边界

- **对应要件：** FR-OPS-005 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：语言切换不改变权限/对象数据；日语信息自然一致。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 095 UT-FR-OPS-006 — 无障碍与认知负荷-纯逻辑与错误边界

- **对应要件：** FR-OPS-006 | 阶段 E2 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：键盘完成桶创建、恢复版本和权限查询，核心流程可用性评审通过。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 096 UT-FR-OPS-007 — CLI与配置即代码-纯逻辑与错误边界

- **对应要件：** FR-OPS-007 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：操作与Web实现相同权限与审计策略。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 097 UT-FR-OPS-008 — API文档与SDK示例-纯逻辑与错误边界

- **对应要件：** FR-OPS-008 | 阶段 E2 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：新开发者按文档完成上传、Git LFS和迁移样例。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 098 UT-FR-OPS-009 — 审计查询与报告-纯逻辑与错误边界

- **对应要件：** FR-OPS-009 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：报告可重现追查一次管理员动作与Git修改。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 099 UT-FR-OPS-010 — 监控与告警-纯逻辑与错误边界

- **对应要件：** FR-OPS-010 | 阶段 E0 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：Grafana/兼容仪表板可读取关键指标并按阈值触发。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 100 UT-FR-OPS-011 — 零停机滚动升级-纯逻辑与错误边界

- **对应要件：** FR-OPS-011 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) N/N-1混部滚动更替Meta voters、数据节点和Gateway，故障在Meta schema迁移中发生；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** quorum不丢、旧版能读新版本记录或进入阻断回退，备份可恢复；对照原要件验收：在允许的单节点轮替中读写持续；失败能按预案恢复。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 101 UT-FR-OPS-012 — 备份与恢复-纯逻辑与错误边界

- **对应要件：** FR-OPS-012 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：仅从备份与对象数据可复原Git映射和用户可读对象。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 102 UT-FR-OPS-013 — 异常与根因信息-纯逻辑与错误边界

- **对应要件：** FR-OPS-013 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：S3错误+后台trace可串联到具体后端与操作。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 103 UT-FR-OPS-014 — 运维 runbook-纯逻辑与错误边界

- **对应要件：** FR-OPS-014 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：演练人员不依赖开发者介入完成预定操作。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 104 UT-FR-OPS-015 — 服务水平报告-纯逻辑与错误边界

- **对应要件：** FR-OPS-015 | 阶段 E1 | 优先级 MUST
- **前提：** 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端
- **测试步骤：** 1) 准备TenantAdmin、Ops、Auditor、Approver各账号、Admin API测试租户、管理Web和minios3ctl仿真客户端；2) 注入Admin Mock/JobLedger/RoleCheck/ConfigRevision，执行管理动作及失败分支；3) 注入同操作的非法/边界输入并重试；4) 提取数据与trace证据。
- **判定：** 危险操作没有不同用户审批不得执行，CLI与Web同鉴权同审计；逐项验收：月报按原始监控数据复算一致。
- **证据：** cargo test / proptest output + coverage + deterministic fixture
- **方法：** CI unit
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 105 UT-NFR-AVA-001 — 生产可用率-指标与质量门禁

- **对应要件：** NFR-AVA-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『E1至少3故障域、正常维护口径按SLA另行约定』下测得『月度 S3 API 可用率≥99.95%（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 106 UT-NFR-AVA-002 — 确认写入持久性-指标与质量门禁

- **对应要件：** NFR-AVA-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任意单节点/单磁盘故障及配置允许的故障数量范围内』下测得『已返回2xx的对象不得丢失，损坏可自动检出』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 107 UT-NFR-AVA-003 — 站点恢复-指标与质量门禁

- **对应要件：** NFR-AVA-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『异地主备配置已启用、依赖服务可用』下测得『生产RTO≤60分钟；异步跨站RPO≤15分钟（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 108 UT-NFR-AVA-004 — 同站数据一致性-指标与质量门禁

- **对应要件：** NFR-AVA-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『针对primary、ACK后单键与LIST可见性』下测得『ACK后GET/HEAD/LIST强一致；跨站异步复制单独声明』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 109 UT-NFR-AVA-005 — WORM恢复安全-指标与质量门禁

- **对应要件：** NFR-AVA-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『磁盘失效、节点回滚、时钟漂移』下测得『保留版本在保留期限内不得通过应用API提前删除』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 110 UT-NFR-AVA-006 — 恢复演练周期-指标与质量门禁

- **对应要件：** NFR-AVA-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态
- **测试步骤：** 1) 3AZ/6节点B1+真实Meta quorum、存储失败注入器，独立观测readable与writable状态；2) 注入单节点/AZ/多数派/时钟/跨站异步中断，记录ACK确认、可读/可写、RPO/RTO及恢复证据；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产变更与季度计划』下测得『季度DR演练；每月备份恢复抽样验证』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 111 UT-NFR-SEC-001 — 权限拒绝-指标与质量门禁

- **对应要件：** NFR-SEC-001 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『任意匿名、过期、跨租户、撤销凭据』下测得『敏感资源默认拒绝；跨租户访问成功数=0』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 112 UT-NFR-SEC-002 — 加密-指标与质量门禁

- **对应要件：** NFR-SEC-002 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『生产外部/东西向连接、静态数据』下测得『TLS1.2+；静态强加密并依赖受控KMS；禁用明文后退』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 113 UT-NFR-SEC-003 — 凭据处理-指标与质量门禁

- **对应要件：** NFR-SEC-003 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『CI/CD、日志、控制台、事故现场』下测得『不能输出密钥明文；密钥轮换和吊销符合审计要求』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 114 UT-NFR-SEC-004 — 漏洞要求-指标与质量门禁

- **对应要件：** NFR-SEC-004 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『正式版本发布前及修复SLAs』下测得『0个已知未接受的Critical漏洞；高危经审批例外』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 115 UT-NFR-SEC-005 — 审计保留-指标与质量门禁

- **对应要件：** NFR-SEC-005 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『关键安全日志含Object Lock/KMS/Git操作』下测得『审计事件可关联且在外部不可变存储受控保留≥365天（草案）』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 116 UT-NFR-SEC-006 — 供应链验证-指标与质量门禁

- **对应要件：** NFR-SEC-006 | 阶段 E1/原SRS | 优先级 MUST
- **前提：** OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查
- **测试步骤：** 1) OIDC/STS/KMS/LegalHold+审计Sink、未授权矩阵、镜像与SBOM检查；2) 执行租户越权、凭据撤销、KMS拒绝、WORM防绕过及供应链发布Gate审查；3) 保存原始测量/失败证据，检查复现；4) 按已合意环境B1/发布指标判断Pass或Blocked。
- **判定：** 在前提『正式发行包、容器及三方依赖』下测得『SBOM齐全、构建物签名、授权许可证扫描完成』；未确认目标值/硬件/样本统计口径时必须标记Needs Agreement，不允许凭单次演示Pass。
- **证据：** raw benchmark CSV + environment bill + chaos timeline + dashboard export + reviewer signature
- **方法：** CI+实验环境
- **执行结果：** Not Run
- **阻断说明：** 实现/环境到位后执行

## 117 UT-ADV-SIGV4-RAW — Host/path双重编码差异

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) RequestTarget = /a//b?x=%252F 与 /a/b?x=%2F，Canonical hash变化，绝不复用签名；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** RequestTarget = /a//b?x=%252F 与 /a/b?x=%2F，Canonical hash变化，绝不复用签名；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 118 UT-ADV-SIGV4-HEAD — 重复Header与folding

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 同名Header不同空格和大小写形式Canon相同/不同情况按AWS标准向量校验；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 同名Header不同空格和大小写形式Canon相同/不同情况按AWS标准向量校验；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 119 UT-ADV-MPU-RACE — Complete与Abort CAS竞争

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 单UploadId只有一个终态，孤儿Part不可见；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 单UploadId只有一个终态，孤儿Part不可见；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 120 UT-ADV-OBJLOCK-TIME — Retention回拨时钟

- **对应要件：** FR-SEC-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 可信Clock倒退时不能减小原生保留期限；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 可信Clock倒退时不能减小原生保留期限；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 121 UT-ADV-OBJLOCK-NULL — Suspended null版本保护

- **对应要件：** FR-SEC-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 现存null Version锁住时禁止覆盖导致的提前物理删；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 现存null Version锁住时禁止覆盖导致的提前物理删；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 122 UT-ADV-LIST-TAMPER — Token字段修改

- **对应要件：** FR-S3-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 修改prefix/snapshot/tenant/expiry后AEAD校验失败；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 修改prefix/snapshot/tenant/expiry后AEAD校验失败；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 123 UT-ADV-RAFT-REPLAY — ReadIndex和log重放

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) term上升时旧term确认不得发布新Head；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** term上升时旧term确认不得发布新Head；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 124 UT-ADV-RAFT-SNAP — Snapshot compaction

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 有活动分页snapshot与GC watermark时索引不被清除；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 有活动分页snapshot与GC watermark时索引不被清除；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 125 UT-ADV-EC-PLACEMENT — 六片故障域组合

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) k4m2所有片不同disk+node，AZ各2，节点不足必须拒绝；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** k4m2所有片不同disk+node，AZ各2，节点不足必须拒绝；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 126 UT-ADV-EC-NONCE — 重试不复用nonce

- **对应要件：** FR-NATIVE-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 不同明文不能用相同key/nonce重复加密；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 不同明文不能用相同key/nonce重复加密；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 127 UT-ADV-GIT-OID — Repo-A猜Repo-B OID

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 存在同OID物理blob但没有repo grant返回NoAccess；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 存在同OID物理blob但没有repo grant返回NoAccess；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 128 UT-ADV-GIT-EVENT — 事件重复与逆序

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 同event_id+ref_seq只推进一次，丢序要pending/reconcile；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 同event_id+ref_seq只推进一次，丢序要pending/reconcile；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 129 UT-ADV-GC-RACE — GC终端复查竞态

- **对应要件：** FR-GG-005 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 候选期新签名Grant到来必须阻止物理删除；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 候选期新签名Grant到来必须阻止物理删除；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 130 UT-ADV-MIG-FENCE — Epoch旧主重放

- **对应要件：** FR-XCHG-006 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 切主后旧epoch Put必须被拒并审计；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 切主后旧epoch Put必须被拒并审计；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 131 UT-ADV-AUDIT-ATOMIC — Outbox同元数据命令提交

- **对应要件：** FR-SEC-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 对象可见与outbox事件不存在单边成功；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 对象可见与outbox事件不存在单边成功；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 132 UT-ADV-JOB-LEASE — Worker旧Lease覆盖

- **对应要件：** FR-OPS-001 | 阶段 E0/E1按场景 | 优先级 P1
- **前提：** 独立隔离测试环境，复用本包攻击fixture、稳定时钟与随机故障种子
- **测试步骤：** 1) 固定初始Bucket/Ref/Version/routeEpoch；2) 旧worker不能用过期lease写Checkpoint；3) 重复10次或以参数化/性质用例探索边界；4) 重启并重新读取权威数据。
- **判定：** 旧worker不能用过期lease写Checkpoint；拒绝路径不能产生ACK写入、越权、违反Retention或未经证实的数据损失，留下证据。
- **证据：** 原始trace+sha/result+failure injection seed+bug reference
- **方法：** property/fuzz or chaos harness
- **执行结果：** Not Run
- **阻断说明：** 需要实际实现和环境才能执行；当前均Not Run

## 133 UT-FR-GG-001 — GitGit仓库权威 — UT

- **对应要件：** FR-GG-001 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“GitGit仓库权威”核心Trait正例断言；3) 注入重复并发CAS、期待旧Ref修订号，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；停用所有外部Git兼容接口，仍能完成创建仓库、提交、分支与历史恢复。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 134 UT-FR-GG-002 — 不可变提交图 — UT

- **对应要件：** FR-GG-002 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“不可变提交图”核心Trait正例断言；3) 注入主节点断电与ACK丢失，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；同输入稳定得到同Commit ID；不能通过正常API改写历史对象。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 135 UT-FR-GG-003 — 原子Ref CAS — UT

- **对应要件：** FR-GG-003 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“原子Ref CAS”核心Trait正例断言；3) 注入重名路径和Unicode规范化，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；两名并发写者仅一个成功；失败者看见明确冲突且不产生部分Ref。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 136 UT-FR-GG-004 — 仓库快照 — UT

- **对应要件：** FR-GG-004 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“仓库快照”核心Trait正例断言；3) 注入Outbox重复/乱序/永久缺失，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；重命名/删除之后checkout旧Commit按字节还原全部文件。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 137 UT-FR-GG-005 — 两阶段提交门禁 — UT

- **对应要件：** FR-GG-005 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“两阶段提交门禁”核心Trait正例断言；3) 注入无权限读、票据过期与重试，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；缺片或Receipt过期时拒绝Ref发布，且保留可补偿事务。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 138 UT-FR-GG-006 — 幂等确认与Outbox — UT

- **对应要件：** FR-GG-006 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“幂等确认与Outbox”核心Trait正例断言；3) 注入断网恢复时Ref已改变，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；断网+重放+乱序事件后两侧谱系最终对账一致，不产生重复版本。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 139 UT-FR-GG-007 — 树级变更与重命名 — UT

- **对应要件：** FR-GG-007 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“树级变更与重命名”核心Trait正例断言；3) 注入Tag保护、ForcePush、旧Snapshot，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；重命名保留AssetId及全部历史；路径冲突不得静默覆盖。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 140 UT-FR-GG-008 — 分支差异查询 — UT

- **对应要件：** FR-GG-008 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“分支差异查询”核心Trait正例断言；3) 注入仓库级ACL撤销与GC竞态，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；带1000个资产的样本差异结果可按AssetId/RevisionId复算。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 141 UT-FR-GG-009 — 原生资产锁 — UT

- **对应要件：** FR-GG-009 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“原生资产锁”核心Trait正例断言；3) 注入重复并发CAS、期待旧Ref修订号，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；并发锁冲突拒绝覆盖，强制解锁有审计并更新fencing token。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 142 UT-FR-GG-010 — 离线工作区日志 — UT

- **对应要件：** FR-GG-010 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“离线工作区日志”核心Trait正例断言；3) 注入主节点断电与ACK丢失，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；断网修改恢复后不误覆盖远端并提示冲突。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 143 UT-FR-GG-011 — 稀疏工作区与懒加载 — UT

- **对应要件：** FR-GG-011 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“稀疏工作区与懒加载”核心Trait正例断言；3) 注入重名路径和Unicode规范化，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；未选中的20GiB媒体不下载，选中内容下载SHA与Manifest一致。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 144 UT-FR-GG-012 — 三方合并引擎 — UT

- **对应要件：** FR-GG-012 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“三方合并引擎”核心Trait正例断言；3) 注入Outbox重复/乱序/永久缺失，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；不可安全合并时返回显式冲突，绝不自动选取后写者。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 145 UT-FR-GG-013 — 冲突决议审计 — UT

- **对应要件：** FR-GG-013 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“冲突决议审计”核心Trait正例断言；3) 注入无权限读、票据过期与重试，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；合并结果可复查全部parent和resolve原因。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 146 UT-FR-GG-014 — 媒体评审流 — UT

- **对应要件：** FR-GG-014 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体评审流”核心Trait正例断言；3) 注入断网恢复时Ref已改变，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；未获必需审批无法发布指定受保护分支。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 147 UT-FR-GG-015 — 依赖变更影响 — UT

- **对应要件：** FR-GG-015 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“依赖变更影响”核心Trait正例断言；3) 注入Tag保护、ForcePush、旧Snapshot，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；修改纹理后定位全部引用该纹理的模型且不跨分支误报。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 148 UT-FR-GG-016 — 历史可达性水位 — UT

- **对应要件：** FR-GG-016 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“历史可达性水位”核心Trait正例断言；3) 注入仓库级ACL撤销与GC竞态，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；丢事件或正在并发恢复时GC fail-closed且不得误删仍可达块。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 149 UT-FR-GG-017 — 组织级授权 — UT

- **对应要件：** FR-GG-017 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“组织级授权”核心Trait正例断言；3) 注入重复并发CAS、期待旧Ref修订号，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；撤权与过期读票据不会绕开资源级ACL。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 150 UT-FR-GG-018 — 受保护提交策略 — UT

- **对应要件：** FR-GG-018 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“受保护提交策略”核心Trait正例断言；3) 注入主节点断电与ACK丢失，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；无权force-update拒绝且所有策略决策留痕。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 151 UT-FR-GG-019 — 标准Git迁入隔离 — UT

- **对应要件：** FR-GG-019 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；双工作区、受保护分支、媒体锁/审查票据、离线journal、带引用历史的依赖DAG
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“标准Git迁入隔离”核心Trait正例断言；3) 注入重名路径和Unicode规范化，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；禁用互操作插件后原生核心可运行；迁移映射差异清晰可查。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 152 UT-FR-GG-020 — 提交与资产恢复 — UT

- **对应要件：** FR-GG-020 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；RepoA与RepoB各2个用户、同名资产、旧/新Commit、并发RefRevision
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“提交与资产恢复”核心Trait正例断言；3) 注入Outbox重复/乱序/永久缺失，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；模拟跨存储迁移后历史恢复字节正确。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 153 UT-FR-MV-001 — 稳定资产标识 — UT

- **对应要件：** FR-MV-001 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“稳定资产标识”核心Trait正例断言；3) 注入Chunk大小边界与文件前段插入，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；移动/改名不改变AssetId，更新内容必生成新Revision。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 154 UT-FR-MV-002 — 媒体版本记录 — UT

- **对应要件：** FR-MV-002 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体版本记录”核心Trait正例断言；3) 注入篡改Chunk/Manifest与重试校验，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；同一Asset存在分叉与合并时可画出完整版本DAG。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 155 UT-FR-MV-003 — 内容清单 — UT

- **对应要件：** FR-MV-003 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“内容清单”核心Trait正例断言；3) 注入跨租户去重存在性推断，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；任意读取块可按清单重组并与原始SHA-256完全一致。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 156 UT-FR-MV-004 — 内容定义分块 — UT

- **对应要件：** FR-MV-004 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“内容定义分块”核心Trait正例断言；3) 注入无法解析媒体并触发隔离限额，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；前部插入小内容后后续大部分相同Chunk能够复用，参数与分块版本固定。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 157 UT-FR-MV-005 — 强摘要验证 — UT

- **对应要件：** FR-MV-005 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“强摘要验证”核心Trait正例断言；3) 注入Adapter升级、能力降级与许可证失败，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；随机篡改任意Chunk或Manifest必被拒绝/隔离，不能返回成功ACK。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 158 UT-FR-MV-006 — 安全租户去重 — UT

- **对应要件：** FR-MV-006 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“安全租户去重”核心Trait正例断言；3) 注入三方merge冲突与并行修改，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；Tenant B测时/响应无法判定Tenant A内容是否存在，B无权取A的Chunk。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 159 UT-FR-MV-007 — 断点续传 — UT

- **对应要件：** FR-MV-007 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“断点续传”核心Trait正例断言；3) 注入S3物理版本变化及迁移Cutover，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；20GiB模拟流断线后仅补传缺块，不重复生成同一可见Revision。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 160 UT-FR-MV-008 — 增量下载 — UT

- **对应要件：** FR-MV-008 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“增量下载”核心Trait正例断言；3) 注入GC/回滚和已ACK资产的并发可达性，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；部分请求字节/offset正确，内存占用不随文件总大小线性增长。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 161 UT-FR-MV-009 — 原件不可变 — UT

- **对应要件：** FR-MV-009 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“原件不可变”核心Trait正例断言；3) 注入Chunk大小边界与文件前段插入，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；所有派生对象sha不同空间，重取旧Commit的原件字节完全一致。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 162 UT-FR-MV-010 — 媒体探测 — UT

- **对应要件：** FR-MV-010 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体探测”核心Trait正例断言；3) 注入篡改Chunk/Manifest与重试校验，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；伪造扩展名不能自动触发高风险解析器，未知类型安全保存。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 163 UT-FR-MV-011 — 可扩展元数据 — UT

- **对应要件：** FR-MV-011 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“可扩展元数据”核心Trait正例断言；3) 注入跨租户去重存在性推断，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；未知字段可往返且Schema升级不破坏原始数据。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 164 UT-FR-MV-012 — 媒体适配器统一接口 — UT

- **对应要件：** FR-MV-012 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体适配器统一接口”核心Trait正例断言；3) 注入无法解析媒体并触发隔离限额，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；不支持能力返回Unsupported而非默认成功，Adapter升级不修改旧Revision。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 165 UT-FR-MV-013 — 图像版本差异 — UT

- **对应要件：** FR-MV-013 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“图像版本差异”核心Trait正例断言；3) 注入Adapter升级、能力降级与许可证失败，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；参考图golden数据可复算热区，PSDs不支持图层时降级为渲染图比较。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 166 UT-FR-MV-014 — 视频时间线差异 — UT

- **对应要件：** FR-MV-014 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“视频时间线差异”核心Trait正例断言；3) 注入三方merge冲突与并行修改，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；变更片段时间码与样本golden一致，不能把非确定性转码当原内容证据。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 167 UT-FR-MV-015 — 音频版本差异 — UT

- **对应要件：** FR-MV-015 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“音频版本差异”核心Trait正例断言；3) 注入S3物理版本变化及迁移Cutover，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；参考音频能定位差异区间，未标准化采样率须显式提示。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 168 UT-FR-MV-016 — 三维资产差异 — UT

- **对应要件：** FR-MV-016 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“三维资产差异”核心Trait正例断言；3) 注入GC/回滚和已ACK资产的并发可达性，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；相同拓扑不同材质及相同外观不同网格被正确区别。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 169 UT-FR-MV-017 — 安全合并能力声明 — UT

- **对应要件：** FR-MV-017 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“安全合并能力声明”核心Trait正例断言；3) 注入Chunk大小边界与文件前段插入，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；两个不可合并二进制冲突绝不自动丢失一方修改。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 170 UT-FR-MV-018 — 资源依赖DAG — UT

- **对应要件：** FR-MV-018 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“资源依赖DAG”核心Trait正例断言；3) 注入篡改Chunk/Manifest与重试校验，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；循环引用检测、跨Commit依赖变更反向追踪正确。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 171 UT-FR-MV-019 — 多文件原子资源快照 — UT

- **对应要件：** FR-MV-019 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“多文件原子资源快照”核心Trait正例断言；3) 注入跨租户去重存在性推断，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；断电时不会出现Commit A包含一半新素材一半未确认素材。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 172 UT-FR-MV-020 — 媒体派生缓存隔离 — UT

- **对应要件：** FR-MV-020 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体派生缓存隔离”核心Trait正例断言；3) 注入无法解析媒体并触发隔离限额，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；缓存污染或过期不影响原始数据，可清除并重建。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 173 UT-FR-MV-021 — 隔离媒体解析 — UT

- **对应要件：** FR-MV-021 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“隔离媒体解析”核心Trait正例断言；3) 注入Adapter升级、能力降级与许可证失败，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；构造ZIP炸弹/格式畸形样本，Worker触发限额时主服务不崩溃。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 174 UT-FR-MV-022 — 媒体权限和分享 — UT

- **对应要件：** FR-MV-022 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体权限和分享”核心Trait正例断言；3) 注入三方merge冲突与并行修改，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；撤权后旧预览URL及Range不能继续访问受保护源。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 175 UT-FR-MV-023 — 资产搜索索引 — UT

- **对应要件：** FR-MV-023 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“资产搜索索引”核心Trait正例断言；3) 注入S3物理版本变化及迁移Cutover，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；权限过滤正确且展示index watermark，允许索引延迟但不伪造提交状态。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 176 UT-FR-MV-024 — 冲突可视化 — UT

- **对应要件：** FR-MV-024 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“冲突可视化”核心Trait正例断言；3) 注入GC/回滚和已ACK资产的并发可达性，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；不支持内容显示元数据差异、哈希和人工冲突入口。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 177 UT-FR-MV-025 — 作品来源与授权 — UT

- **对应要件：** FR-MV-025 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“作品来源与授权”核心Trait正例断言；3) 注入Chunk大小边界与文件前段插入，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；授权过期的外部媒体阻断发布，下载与审计保留可授权访问。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 178 UT-FR-MV-026 — 可验证媒体导出 — UT

- **对应要件：** FR-MV-026 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“可验证媒体导出”核心Trait正例断言；3) 注入篡改Chunk/Manifest与重试校验，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；导出的包在离线验证工具中重建SHA与版本谱系一致。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 179 UT-FR-MV-027 — 存储逻辑与业务版本隔离 — UT

- **对应要件：** FR-MV-027 | 阶段 E0 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“存储逻辑与业务版本隔离”核心Trait正例断言；3) 注入跨租户去重存在性推断，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；迁移后GitGit revision不改变，即使目标Provider versionId完全不同。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 180 UT-FR-MV-028 — 安全垃圾回收 — UT

- **对应要件：** FR-MV-028 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“安全垃圾回收”核心Trait正例断言；3) 注入无法解析媒体并触发隔离限额，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；在并发checkout/restore与GC竞态中绝不删仍有引用的Chunk。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 181 UT-FR-MV-029 — 块级迁移 — UT

- **对应要件：** FR-MV-029 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；固定SHA的20GiB惰性媒体流、Chunk/Manifest golden、断线插入1KiB对照
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“块级迁移”核心Trait正例断言；3) 注入Adapter升级、能力降级与许可证失败，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；切换期间所有受保留Commit可恢复，迁移后块SHA不变。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 182 UT-FR-MV-030 — 媒体协作事件 — UT

- **对应要件：** FR-MV-030 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“媒体协作事件”核心Trait正例断言；3) 注入三方merge冲突与并行修改，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；单资产事件乱序处理后最终timeline与权威Commit序列一致。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 183 UT-FR-MV-031 — 批量导入与目录映射 — UT

- **对应要件：** FR-MV-031 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“批量导入与目录映射”核心Trait正例断言；3) 注入S3物理版本变化及迁移Cutover，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；导入重试不会重复资产；目录相对路径和大小写冲突按策略报告。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 184 UT-FR-MV-032 — 面向生产的预览代理 — UT

- **对应要件：** FR-MV-032 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“面向生产的预览代理”核心Trait正例断言；3) 注入GC/回滚和已ACK资产的并发可达性，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；代理生成失败不阻止GitGit Checkout；展示派生版本与处理参数。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 185 UT-FR-MV-033 — 复杂工程资源类型 — UT

- **对应要件：** FR-MV-033 | 阶段 E2 | 优先级 SHOULD
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“复杂工程资源类型”核心Trait正例断言；3) 注入Chunk大小边界与文件前段插入，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；缺少某Adapter不影响完整二进制保存，依赖分析显式标Unsupported。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可

## 186 UT-FR-MV-034 — 资产语义标签 — UT

- **对应要件：** FR-MV-034 | 阶段 E1 | 优先级 MUST
- **前提：** 固定时钟、FakeMeta/CAS/Policy、确定性资产golden；PNG/TIFF、MP4/MOV、WAV/FLAC、GLB/FBX、未知binary和恶意畸形文件的golden集
- **测试步骤：** 1) 建立固定的Rust Domain/状态机Fixture并计算基础输出；2) 调用“资产语义标签”核心Trait正例断言；3) 注入篡改Chunk/Manifest与重试校验，校验ErrorKind及事务前后状态；4) 用proptest重复乱序与边界值1000组，输出失败seed与coverage。
- **判定：** 返回类型/序列化/状态变换满足DD定义，不可变对象和租户作用域不破坏；可查询某Commit下标签快照，后改标签不篡改提交历史。
- **证据：** cargo test --workspace + proptest seed + assertions + coverage JSON
- **方法：** CI unit + property
- **执行结果：** Not Run
- **阻断说明：** 需对应功能真实代码及环境；媒体golden须记录版本及许可
