# UT-MINIOS3-002 — 单元测试规格补充、Fixture与覆盖口径

> 权威设计：`../../05_test_design/TD-MINIOS3-UT-001_单元测试设计书_v2.0.md`；案例：`UT_完整案例明细_v2.0.md`。

## 1. 测试分类与输入条件
| Unit | 关键输入 | 核心断言 | 失败注入 |
|---|---|---|---|
| Manifest codec | 空文件、1B、chunk边界、20GiB模拟流、非法offset | CanonicalHash稳定，大小与SHA256一致 | 缺口/重叠/重复块 |
| RefStore | 多父Commit、旧Ref revision、并发CAS | 只有一个CAS成功且冲突无副作用 | ACK丢失、重放 |
| Reachability | Branch、Tag、LegalHold、danglingRef | 不可判定时拒删 | 图缺边/环/混乱事件 |
| S3 signer | Unicode Key、空query、特殊字符 | AWS SigV4向量一致 | 签名过期、规范化差异 |
| Chunking | 重复媒体、轻微前插、压缩视频 | 固定数据确定性切块、完整恢复 | chunk长度溢出 |
| Auth engine | 跨租户/角色/PolicyEpoch变化 | 默认拒绝 | 缓存失效/角色撤销 |
| MediaAdapter | 未识别格式、损坏样本、受限资源 | 降级而不触碰原件 | Parser crash/Timeout |

## 2. Golden Fixture来源规则
Fixture记录`fixture_id, format, bytes_sha256, logical_size, license, expected_manifest_digest, expected_chunks, expected_conflicts`。大文件可采用确定性生成脚本，需记录seed并在CI校验哈希；不能把机密素材放入仓库。跨格式黄金用例须包含无媒体插件时按字节原样存储。

## 3. 覆盖统计
行覆盖、分支覆盖、Rust状态机转换覆盖和需求覆盖分开统计。重点不变量必须有正反例，所有已承诺UT案例运行结果来自CI，当前全部Not Run。覆盖率阈值需由项目质量目标批准后冻结，不通过人工标注伪造。
