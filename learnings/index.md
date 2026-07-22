# Learnings Index

| Learning | Area | Summary |
|---|---|---|
| [sdd-knowledge-sync-branch-mismatch](sdd-knowledge-sync-branch-mismatch.md) | ci, tooling | knowledge-sync.yml is installed with `branches: [main]` hardcoded — must fix to `master` for repos that use master as default |
| [scip-go-unavailable-ast-fallback](scip-go-unavailable-ast-fallback.md) | tooling, kb-bootstrap | SCIP indexer unavailable when Go toolchain is absent in the KB bootstrap pod; AST path still runs correctly |
