---
category: observability
subcategory: metrics
confidence: high
documentType: reference
scope: org
contentHash: b5c7083b3461
tags: [metrics]
source: README.md
verified: 2026-07-22
---

## Configuration

The exporter can be configured using env variables or command flags.

| **KEY** | **description** |
|-|-|
| `CF_API_EMAIL` |  user email (see https://support.cloudflare.com/hc/en-us/articles/200167836-Managing-API-Tokens-and-Keys) |
| `CF_API_KEY` |  API key associated with email (`CF_API_EMAIL` is required if this is set)|
| `CF_API_TOKEN` |  API authentication token (recommended before API key + email. Version 0.0.5+. see https://developers.cloudflare.com/analytics/graphql-api/getting-started/authentication/api-token-auth) |
| `CF_ZONES` |  (Optional) cloudflare zones to export, comma delimited list of zone ids. If not set, all zones from account are exported |
| `CF_EXCLUDE_ZONES` |  (Optional) cloudflare zones to exclude, comma delimited list of zone ids. If not set, no zones from account are excluded |
| `FREE_TIER` | (Optional) scrape only metrics included in free plan. Accepts `true` or `false`, default `false`. |
| `LISTEN` |  listen on addr:port (default `:8080`), omit addr to listen on all interfaces |
| `METRICS_PATH` |  path for metrics, default `/metrics` |
| `SCRAPE_DELAY` | scrape delay in seconds, default `300` |
| `CF_BATCH_SIZE` | cloudflare request zones batch size (1 - 10), default `10` |
| `METRICS_DENYLIST` | (Optional) cloudflare-exporter metrics to not export, comma delimited list of cloudflare-exporter metrics. If not set, all metrics are exported |
| `ZONE_<NAME>` |  `DEPRECATED since 0.0.5` (optional) Zone ID. Add zones you want to scrape by adding env vars in this format. You can find the zone ids in Cloudflare dashboards. |

Corresponding flags:
```
  -cf_api_email="": cloudflare api email, works with api_key flag
  -cf_api_key="": cloudflare api key, works with api_email flag
  -cf_api_token="": cloudflare api token (version 0.0.5+, preferred)
  -cf_zones="": cloudflare zones to export, comma delimited list
  -cf_exclude_zones="": cloudflare zones to exclude, comma delimited list
  -free_tier=false: scrape only metrics included in free plan, default false
  -listen=":8080": listen on addr:port ( default :8080), omit addr to listen on all interfaces
  -metrics_path="/metrics": path for metrics, default /metrics
  -scrape_delay=300: scrape delay in seconds, defaults to 300
  -cf_batch_size=10: cloudflare zones batch size (1-10)
  -metrics_denylist="": cloudflare-exporter metrics to not export, comma delimited list
```

Note: `ZONE_<name>` configuration is not supported as flag.

## See Also
- [run](../../ci/workflows/run.md) <!-- rel:strong -->
- [authentication](../../security/auth/authentication.md) <!-- rel:strong -->
- [build](../../build/build.md) <!-- rel:related -->
- [license](../../libs/license.md) <!-- rel:related -->
- [contributing and reporting issues](../../code-conventions/git/contributing-and-reporting-issues.md) <!-- rel:related -->
