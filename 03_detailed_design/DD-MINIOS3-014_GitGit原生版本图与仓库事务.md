# DD-MINIOS3-014 — GitGit Rust原生版本图/Ref/Commit/Workspace

> v2.0 | 模块权威 GitGit Core | 关联FR-GG-001～020

## 1. Rust crates与API边界

`gitgit-domain`强类型ID/验证；`gitgit-graph` DAG/Tree/Commit canonical编码；`gitgit-ref-store` Ref CAS；`gitgit-workspace` checkout、status、rename；`gitgit-commit-coordinator` receipts/Outbox；`gitgit-collaboration` Lock/Review/Merge。MinIOS3持有Manifest/Chunk而非Ref状态。

```rust
// 设计草案，不是可编译的现有代码
struct CommitRecord { id: CommitId, parents: Vec<CommitId>, root_tree: TreeId,
    author: PrincipalId, created_at: LogicalTime, schema: u16 }
struct AssetEntry { path: RepoPath, asset_id: AssetId,
    revision_id: RevisionId, manifest_id: ManifestId }
struct RefCas { repo: RepoId, name: RefName, expected: RefRevision,
    new_head: CommitId, txn: TxnId, receipt_set_digest: Digest }
trait RefStore { async fn compare_and_swap(&self, cmd: RefCas)
    -> Result<CommittedRef, RefConflict>; }
```

## 2. 稳定ID与确定性编码

Commit ID根据版本化canonical编码SHA-256生成；`tree_id`对按规范路径字节排序的TreeEntry编码求hash；AssetId表示逻辑资产身份、RevisionId表示父版本及内容变更。跨平台路径采用明确的大小写/Unicode策略，禁止目录穿越、保留路径与不规范路径冲突；不能仅以路径重命名识别AssetId。

## 3. 原子提交算法

接收`NewCommit{parents,root_tree,ref,expected_revision,manifest_refs,idempotency_key}`→遍历依赖校验不存在悬空Revision/Commit→调用MinIOS3 verify sealed receipts→GitGit本地事务Append Commit+RefCAS+Outbox→返回包含`ref_revision`的Result。Crash前后根据幂等键恢复唯一结果。权限PolicyEpoch/ReceiptPlacementEpoch在CAS点二次验证。

## 4. Branch/Tag与Merge

FastForward检查DAG祖先关系；non-FF要求显式权限及MergeCommit；三方比较`base,left,right`，按AssetId/路径映射rename、删除改动/双端改动冲突；内容自动合并仅调用声明安全的Adapter。未知二进制冲突保持两个候选，不把最后提交当胜者。

## 5. 原生Protocol和兼容边界

原生JSON/gRPC `/v1/gitgit/*` 自有稳定version字段；S3与LFS传输应由内容服务而非GitGit提交层承担。外部Git pack、commit格式、LFS协议通过独立可禁用Adapter，任何互操作失败不可破坏原生GitGit数据。

## 6. 测试准则

状态机model checking: 并发Ref CAS只有单个成功；Commit DAG无环/稳定hash；checkout任意受保留Commit、文件重命名与离线冲突; 真实IT验证崩溃窗口。
