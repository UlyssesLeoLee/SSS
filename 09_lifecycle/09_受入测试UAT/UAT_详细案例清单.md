# UAT-MINIOS3-002 — 受入测试详细案例清单

> 状态：Design Only / Not Run；用例必须在业务代表授权的真实或脱敏样本上执行。

| Case ID | 业务角色 | 操作 | 关联UC | 完成判定 | 阶段 | 结果 |
|---|---|---|---|---|---|---|
| UAT-001 | 视频制作人 | 导入10GiB视频，切换两次Commit并取回原件 | UC-GG-01 | 下载SHA256与两个历史原件各自完全一致 | E0 | Not Run |
| UAT-002 | 图片设计师 | 导入PSD/PNG，查询变更，叠加对比 | UC-02 | 原件hash不变，插件失效时退化为hash/size对比 | E1 | Not Run |
| UAT-003 | 声音制作人 | 两版WAV切换试听并比较时段差异 | UC-GG-02 | 声道/时间码正确，未支持格式明确降级 | E2 | Not Run |
| UAT-004 | 3D美术 | 提交GLB并查看材质、网格差异 | UC-GG-03 | 对比来源明确，不能安全合并时不会覆盖 | E2 | Not Run |
| UAT-005 | 游戏资产主管 | 修改贴图并查询受影响模型 | UC-GG-04 | 依赖图能定位受影响AssetRevision | E1 | Not Run |
| UAT-006 | 双人协作 | 相同资产在两个分支并行修改后合并 | UC-GG-05 | 冲突显式可裁决，RefCAS无静默覆盖 | E1 | Not Run |
| UAT-007 | 远程制作人 | 离线部分克隆后修改、回连并同步 | UC-GG-06 | 增量上传仅传缺块，冲突有可复现提示 | E1 | Not Run |
| UAT-008 | 主美审批 | 提交资源并经Review后发布标签 | UC-GG-09 | 未批准分支无法发布，审批与Commit绑定 | E1 | Not Run |
| UAT-009 | 运维管理员 | 进行S3双后端迁移与回滚演练 | UC-03 | 旧endpoint不变、历史hash一致，未追平禁止切换 | E1 | Not Run |
| UAT-010 | 合规负责人 | 为版本设Legal Hold后尝试删除 | UC-05 | 所有受保护版本拒绝删除且审计可查 | E1 | Not Run |
| UAT-011 | GitGit管理员 | 禁用Git LFS适配，完成完整提交流程 | UC-01 | 原生Commit、Checkout、增量上传完全可用 | E0 | Not Run |
| UAT-012 | 安全审计员 | 撤销用户权限后复用旧token/URL | UC-GG-08 | 超权限读取失败且审计含request-id | E1 | Not Run |
| UAT-013 | 存储运维 | 误删Branch并发GC及恢复历史 | UC-GG-10 | 仍受保护内容均可重新检出 | E1 | Not Run |
| UAT-014 | S3客户端用户 | 通过AWS SDK对统一端点PUT/GET/Range | UC-07 | 支持声明的S3能力，Hash和Range响应正确 | E0 | Not Run |
| UAT-015 | 制作管理员 | 生成代理视频并回溯原件和工具版本 | UC-GG-01 | 每个派生资产指向稳定源OID、版本、参数 | E2 | Not Run |
| UAT-016 | DevOps | 断外网，单机GitGit+MinIOS3完整业务闭环 | UC-GG-01 | 不依赖第三方在线MinIO/AWS/外部Git组件 | E0 | Not Run |

## 一般执行步骤模板
1. 记录操作者/租户/Repo/Build SHA、对象 SHA-256 及预期Ref。
2. 设置原始资产与修改资产；保存基线截图/Manifest摘要和版本映射。
3. 执行本案例业务操作；如含安全拒绝和冲突场景，执行相应负例。
4. 比较预期与实际，记录requestId、日志、完整hash、视频时间码或3D差异范围。
5. 创建缺陷单并按P0/P1/P2闭环，业务人员签署Pass/Fail/Blocked。

## 特殊说明
UAT视频/音频/3D语义差异以对应已发布MediaAdapter能力为前提；不支持的格式不得以“完全相同”通过。
