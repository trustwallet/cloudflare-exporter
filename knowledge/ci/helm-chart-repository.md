---
category: ci
confidence: low
documentType: how-to
scope: org
contentHash: f92696ef3fa0
tags: [deploy]
source: README.md
verified: 2026-07-22
---

## Helm chart repository

To deploy the exporter into Kubernetes, we recommend using our manager Helm repository:

```
helm repo add cloudflare-exporter https://lablabs.github.io/cloudflare-exporter/
helm install cloudflare-exporter/cloudflare-exporter
```

## See Also
- [description](../observability/metrics/description.md) <!-- rel:strong -->
- [contributing and reporting issues](../code-conventions/git/contributing-and-reporting-issues.md) <!-- rel:strong -->
- [build](../build/build.md) <!-- rel:strong -->
- [grafana dashboard](../observability/metrics/grafana-dashboard.md) <!-- rel:strong -->
- [configuration](../observability/metrics/configuration.md) <!-- rel:strong -->
