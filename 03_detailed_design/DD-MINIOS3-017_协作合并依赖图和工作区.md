# DD-MINIOS3-017 — GitGit协作、冲突合并、依赖图与工作区

> v2.0 | 关联FR-GG-009～018、FR-MV-017/018/024/025/030

## 1. 资产工作区状态

`Untracked / Clean / Modified / Staged / Locked / Conflicted / ReviewPending / Approved / Published`为不同维度状态，不使用互斥单字段遗漏组合。工作区journal绑定`base_commit, asset_revision, operation_id`，离线重放遇Ref CAS冲突启动三方比较。

## 2. 锁模型

锁键`(tenant,repo,asset_id,scope)`；`LockLease(lock_id,holder,issued_at,expires_at,fencing_epoch,policy_epoch)`。续约和释放需当前fencing epoch；已失效客户端的旧Lease不得写入。硬锁为特定分支和资产的提交Policy，不仅是UI提示。

## 3. 合并规则

`base,left,right`按稳定AssetId匹配；rename vs modify、delete vs modify、modify vs modify、二进制冲突、依赖图循环全部有显式ErrorKind。媒体Adapter只有声明该Schema的safe_merge且返回所有parents/provenance时才允许自动合并，否则人工决议产生新Revision。

## 4. Dependency DAG

`DependencyEdge {from_revision,to_revision,kind,source_adapter_version,observed_at,scope}`，同一Commit内依赖有固定版本约束；index消费者不在没有证据时推断不存在依赖。循环检测使用Tarjan SCC及用户标记，不能通过删除边隐藏循环。

## 5. Review流程

`Draft→InReview→Approved|Rejected→Published`，审批关联不可变Commit/Revision与策略版本；受保护分支执行检查权限与签名审批证据。评论可追加但不能改历史Commit。

## 6. 测试

UT合并冲突表/锁租约模型；IT双工作区离线回放、Review与提交门禁、dependency事件乱序；ST模拟游戏项目大量贴图/模型关系、多人对同资产改动。
