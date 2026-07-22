---
title: "SCIP indexer unavailable when Go toolchain absent — AST path still works"
date: 2026-07-22
pr: TBD
area: [tooling, kb-bootstrap]
files: [.github/workflows/knowledge-sync.yml]
symptom: scip-go fails silently with 'go: command not found' in KB bootstrap pod; no index.scip.json produced
tags: [scip, scip-go, go, kb-bootstrap, ast, code-intelligence]
summary: The KB bootstrap pod may not have the Go toolchain installed, making scip-go unavailable. The AST-only path in sdd-knowledge --full still runs correctly and produces the call graph; SCIP would have resolved member-call edges but is not required.
---

# SCIP indexer unavailable when Go toolchain absent

## The bug / The pattern

The KB bootstrap step 2.5 attempts to run `scip-go` to produce a type-resolved call graph.
On pods without the Go toolchain installed, `go install github.com/sourcegraph/scip-go/...`
fails with `go: command not found`, so no `index.scip.json` is produced.

`sdd-knowledge --full` falls back to the AST-only path, which:
- Still produces the complete call graph for directly-called (lexically-resolved) functions
- Withholds member calls (`obj.method()`) where the receiver type is ambiguous
- Records these as `INFERRED` edges when the method name is unique repo-wide

## The rule

This is expected and non-blocking. The SCIP index is best-effort (step 2.5 says so
explicitly). The AST path is sufficient for a small, non-OOP Go exporter like
cloudflare-exporter where most call paths are direct function calls.

If higher call-graph fidelity is needed, ensure the Go toolchain is installed in the
bootstrap environment before running `sdd-knowledge --full`.

## Detection

In the bootstrap log, look for `go: command not found` or `scip-go install failed`. The
fallthrough note in the progress output should say "SCIP unavailable — step 3 will use AST path."

## Related

- Step 2.5 of the KB bootstrap spec: SCIP is always attempted but never blocks the run.
