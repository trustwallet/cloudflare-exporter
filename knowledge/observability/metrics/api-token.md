---
category: observability
subcategory: metrics
confidence: medium
documentType: reference
scope: org
contentHash: a71e1f9e07ad
tags: [metrics, dashboard]
source: README.md
verified: 2026-07-22
---

## API token

The preferred way of authenticating is with an API token, for which the scope can be configured at the Cloudflare
dashboard.

Required authentication scopes:
- `Analytics:Read` is required for zone-level metrics
- `Account.Account Analytics:Read` is required for Worker metrics
- `Account Settings:Read` is required for Worker metrics (for listing accessible accounts, scraping all available
  Workers included in authentication scope)

To authenticate this way, only set `CF_API_TOKEN` (omit `CF_API_EMAIL` and `CF_API_KEY`)

## See Also
- [run](../../ci/workflows/run.md) <!-- rel:strong -->
- [authentication](../../security/auth/authentication.md) <!-- rel:strong -->
- [list of available metrics](../../ci/workflows/list-of-available-metrics.md) <!-- rel:related -->
- [build](../../build/build.md) <!-- rel:related -->
- [license](../../libs/license.md) <!-- rel:weak -->
