---
category: code-conventions
subcategory: git
confidence: medium
documentType: tutorial
scope: org
contentHash: fe2e66c88b15
tags: [commit, branch]
source: README.md
verified: 2026-07-22
---

## Contributing and reporting issues

Feel free to create an issue in this repository if you have questions, suggestions or feature requests.

### Validation, linters and pull-requests

We want to provide high quality code and modules. For this reason we are using
several [pre-commit hooks](.pre-commit-config.yaml) and
[GitHub Actions workflow](.github/workflows/golangci-lint.yml). A pull-request to the
master branch will trigger these validations and lints automatically. Please
check your code before you will create pull-requests. See
[pre-commit documentation](https://pre-commit.com/) and
[GitHub Actions documentation](https://docs.github.com/en/actions) for further
details.

## See Also
- [build](../../build/build.md) <!-- rel:strong -->
- [list of available metrics](../../ci/workflows/list-of-available-metrics.md) <!-- rel:strong -->
- [helm chart repository](../../ci/helm-chart-repository.md) <!-- rel:strong -->
- [run](../../ci/workflows/run.md) <!-- rel:related -->
- [license](../../libs/license.md) <!-- rel:related -->
