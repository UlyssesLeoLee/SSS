# BR-MINIOS3-001 — 业务需求、作业流程与To-Be业务设计

## 业务过程1：媒体制作循环
1. 制作人导入媒体，客户端计算流式摘要及Chunk Manifest；MinIOS3仅为经授权用户保存内容。
2. 内容上传成功后颁发DurableReceipt；GitGit校验Receipt、PolicyEpoch、Repo授权及Ref旧版本，才提交Commit并原子Ref CAS。
3. 制作人比较两个Commit的媒体差异；Parser不可用时展示原文哈希/大小差异，不作伪精确的结构比较。
4. Reviewer签署审查、可选Protected Branch，发布Snapshot；旧版不可变且可还原。
5. 分支清理进入GC候选，检查Reachability、保留期、WORM、争用栅栏后物理回收。

## 业务过程2：跨S3存储互换
管理员发起Dry-run→导出对照清单（逻辑/物理VersionId映射）→增量复制→校验SHA-256、对象数量、版本与Retention属性→停止或栅栏写入→同步尾差→正式Cutover→持续监视→允许经验证的反向同步回退。不能仅更换后端URL就宣称迁移成功。

## 业务过程3：多人协作
AssetId保持稳定；同一AssetRevision在两个工作区修改时根据预期Ref和MergeBase检测并发。文本/结构化媒体仅由声明supports_safe_merge=true的隔离适配器合并，未知二进制产生显式ConflictRecord。

## 用户角色／授权职责
| 角色 | 主动作 | 禁止/审核 |
|---|---|---|
| Producer | 提交、比较、下载、自有工作区修改 | 不得绕过Protected Branch规则 |
| Reviewer | 审查、批准、拒绝、锁冲突裁决 | 审批记录不得篡改 |
| Repo Admin | 仓库权限、Tag/分支策略 | 不得突破WORM法务保留 |
| Storage Admin | Bucket/路由/副本/配额 | 不得重写GitGit Ref历史 |
| Security Auditor | 审计查看、证据导出 | 原则上不具备内容读取权 |
| Service Account | 限定范围的媒体Worker任务 | 不得访问其他租户Key/KMS |

## 业务规则编号
BR-01：任何已确认Ref都应能还原对应不可变原件；BR-02：Preview故障不得影响原件；BR-03：生产历史不得无审计被清除；BR-04：无完整证据不得声称S3全面兼容；BR-05：对商业资源必须保留来源/许可审批信息。
