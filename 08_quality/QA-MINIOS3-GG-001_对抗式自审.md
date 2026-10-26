# QA-MINIOS3-GG-001 — 多媒体版本管理对抗式自审

> 2026-10-26 | v2.0 | **静态设计审查，不是代码测试**

## 一、文档静态结果

- 旧基线104 FR、32 NFR、12 UC不重号保留，旧FR修订15项。
- 新增54 FR、12 NFR、10 UC；最终为 158 FR、44 NFR、22 UC。
- UT 186、IT 194、ST 249；总设计案例629，执行数0、全部Not Run。

## 二、阻断风险 / 残余审查项目

| 编号 | 严重度 | 风险 | 设计对策 | 真正关闭的证据 |
|---|---|---|---|---|
| GG-R01 | P0 | GitGit Ref CAS前内容未Durable | 签署Manifest receipt并在CAS点核查 | kill-9/掉电IT、零缺块Commit |
| GG-R02 | P0 | ACK丢失后重试生成多个可见Commit | 幂等key、事务结果查询、Outbox同事务 | 并发/重试真实集群IT |
| GG-R03 | P0 | CDC/Chunk哈希或offset漂移造成静默错误 | canonical Manifest+全文件SHA | 20GiB流与golden回放 |
| GG-R04 | P0 | 跨租户去重泄露存储存在性 | 加密域隔离+不暴露dedup结果 | side-channel负例安全测试 |
| GG-R05 | P0 | GC与新Ref并发误删Chunk | signed reachability + GC epoch +二次校验 | 持续并发GC/restore故障注入 |
| GG-R06 | P0 | 3D/视频自动合并不可逆破坏数据 | 默认binary conflict、adapter需声明safe_merge | 双人冲突ST与Hash保存 |
| GG-R07 | P0 | 媒体解析器执行不受信代码 | 独立Worker，禁Secret和网络 | 恶意媒体/逃逸测试 |
| GG-R08 | P0 | S3迁移丢新ACK版本 | RouteEpoch栅栏，回滚前反向追平 | 模拟cutover后新写+双向校验 |
| GG-R09 | P1 | 权威ID混淆导致历史无法还原 | 独立GitGit/Manifest/S3/Provider IDs | 数百万版本导出/恢复验证 |
| GG-R10 | P1 | 大压缩媒体无法实现高去重率 | CDC/Fixed profiling及退化策略 | 真实格式样本测试报告 |
| GG-R11 | P1 | Git LFS兼容压过GitGit原生核心 | 明确LFS仅E1可选适配 | 禁用兼容仍通过E0原生ST |
| GG-R12 | P1 | OSS组件被误认为无需任何许可审查 | 全依赖锁定+SBOM+NOTICE+FFmpeg构建审计 | 发行CI license gate通过 |
| GG-R13 | P0 | WORM保护对象与GitGit GC冲突 | version retention优先+可达性与法务保留 | WORM违规删除0 |
| GG-R14 | P0 | Raft多分片List快照一致性未证明 | 单Bucket单分片默认 | 正式多分片证明前禁止该功能GA |
| GG-R15 | P1 | 离线并发更新和rename造成静默覆盖 | stable AssetId、Ref CAS、冲突UI | 真实双工作区ST |
| GG-R16 | P1 | 历史媒体Adapter更新改变旧Commit解释 | 适配器版本化、原始哈希权威 | 更新Adapter前后Checkout逐字节一致 |

## 三、门禁签字缺口

GitGit真实Rust接口、编译可行性、OpenAPI运行联调、20GiB性能基准、FIPS/WORM合规边界、许可证/专利最终清单、真实部署成本均未验证。所有设计与测试必须仍标Draft/Not Run。
