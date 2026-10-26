# SEC-MINIOS3-001 — 威胁模型 / 安全方案 / 验证标准

## 1. 资产与信任边界
资产：GitGit Commit/Ref权威库、Manifest/Chunk、租户凭据、KMS密钥、审计记录、媒体预览任务。信任边界：浏览器↔Gateway、GitGit↔MinIOS3跨服务Receipt、Raft成员↔数据节点、MinIOS3↔外部S3、Worker↔不受信媒体。

## 2. 威胁与防护
| Threat | 风险 | 控制 | 证明 |
|---|---|---|---|
| 跨租户对象枚举 | 保密资源泄露 | Tenant域隔离、资源级policy、opaque ID | 负例授权ST |
| 跨域去重推断 | 侧信道 | Dedup以授权域为边界、不返回命中状态 | 推断攻击负例 |
| Receipt伪造/重放 | 无持久化仍可Commit | 验证签名、Txn、Expiry、PolicyEpoch | fuzz + Replay IT |
| Ref冲突绕过 | 静默覆盖 | Ref CAS、预期Revision、审计 | 并发IT |
| Worker恶意解析 | RCE/SSRF/密钥窃取 | 非特权隔离worker、无密钥、禁出网、限资源 | sandbox渗透测试 |
| S3签名绕过 | 未授权写入 | SigV4向量、拒签、时钟验证 | SDK矩阵 |
| WORM管理员越权 | 法务保留破坏 | 版本级保护、fail-closed、不可修改审计 | Policy负例ST |
| 供应链替换 | 投毒/恶意依赖 | Cargo.lock/SBOM/签名/漏洞扫描 | Release evidence |

## 3. 许可证与合规
核心使用宽松许可证组件为目标；开源并非免法律义务，AGPL/LGPL/FFmpeg codec/专利等需逐构建功能审查。法规WORM证明不能由代码测试单独替代第三方/法务评估。
