# IMPL-MINIOS3-002 — CI/CD流水线和制品签发规格

## 1. 阶段门禁
`format→lint→dependency audit/license→unit/property→build→API schema/lint→integration→S3 contract→chaos/ST→security review→SBOM+sign→release candidate`。
PR须验证设计ID/需求ID/测试ID有更新；不能只要求代码通过而忽略文档变化。

## 2. Artifact provenance
构建清单包含GitGit commit-id、MinIOS3 commit-id、Rust toolchain版本、Cargo.lock摘要、全部启用features、平台/架构、容器基础镜像digest、构建时间、SBOM路径及签名。将CI build hash与部署实例关联，不在制品内夹带生产密钥。

## 3. Release环节
E0/E1/E2按独立功能集合发布；回归的UT/IT/ST为阶段门禁，而不是同一天完成全部。发布需产品/QA/Security/SRE签署，P0不允许豁免；P1需明确补偿与到期日。部署前备份+恢复验证+回退开关，WORM与Schema升级不可逆部分单独审查。

## 4. 密钥
CI分支PR无生产密钥，凭证以短期OIDC或临时注入；离线部署制品须签名验证；外部依赖缓存的离线归档不得混入受限许可二进制。
