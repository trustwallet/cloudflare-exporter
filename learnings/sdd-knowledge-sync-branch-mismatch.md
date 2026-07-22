---
title: "knowledge-sync.yml installed with hardcoded 'main' branch — must fix for master-default repos"
date: 2026-07-22
pr: TBD
area: [ci, tooling, kb-bootstrap]
files: [.github/workflows/knowledge-sync.yml]
symptom: knowledge-sync workflow never fires on push to master because sdd-init installs it with `branches: [main]`
tags: [knowledge-sync, sdd-init, branch, main, master, workflow]
summary: sdd-knowledge --full installs knowledge-sync.yml with branches:[main] hardcoded regardless of the repo's actual default branch; repos using master need a manual fix to both the push trigger and the gh pr create --base flag.
---

# knowledge-sync.yml installed with hardcoded 'main' branch

## The bug / The pattern

`sdd-knowledge --full` (or `sdd-init`) installs `.github/workflows/knowledge-sync.yml` with
two hardcoded references to `main`:

1. `on: push: branches: [main]` — the push trigger
2. `gh pr create --base main` — the PR creation command

For any repo whose default branch is `master` (rather than `main`), the sync workflow will
never fire on push, and the garden PR will target the wrong base branch.

## The rule

After running `sdd-knowledge --full` on a repo, check the repo's actual default branch:

```bash
git remote show origin | grep 'HEAD branch'
```

If the result is `master`, fix both occurrences in `.github/workflows/knowledge-sync.yml`:

```bash
sed -i 's/branches: \[main\]/branches: [master]/g' .github/workflows/knowledge-sync.yml
sed -i 's/--base main/--base master/g' .github/workflows/knowledge-sync.yml
```

Commit this as a separate `fix(kb):` commit before opening the KB bootstrap PR.

## Detection

Grep for `[main]` in any newly-installed knowledge-sync.yml on a repo where
`git symbolic-ref refs/remotes/origin/HEAD` resolves to `master`.

## Related

- See PR for cloudflare-exporter KB bootstrap where this was caught and fixed in the
  pre-PR review step.
