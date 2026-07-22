---
category: ci
subcategory: workflows
confidence: medium
documentType: how-to
scope: org
contentHash: bb22a7446243
tags: [script]
source: README.md
verified: 2026-07-22
---

## List of available metrics

```
# HELP cloudflare_worker_cpu_time CPU time quantiles by script name
# HELP cloudflare_worker_duration Duration quantiles by script name (GB*s)
# HELP cloudflare_worker_errors_count Number of errors by script name
# HELP cloudflare_worker_requests_count Number of requests sent to worker by script name
# HELP cloudflare_zone_bandwidth_cached Cached bandwidth per zone in bytes
# HELP cloudflare_zone_bandwidth_content_type Bandwidth per zone per content type
# HELP cloudflare_zone_bandwidth_country Bandwidth per country per zone
# HELP cloudflare_zone_bandwidth_ssl_encrypted Encrypted bandwidth per zone in bytes
# HELP cloudflare_zone_bandwidth_total Total bandwidth per zone in bytes
# HELP cloudflare_zone_colocation_edge_response_bytes Edge response bytes per colocation
# HELP cloudflare_zone_colocation_visits Total visits per colocation
# HELP cloudflare_zone_colocation_requests_total Total requests per colocation
# HELP cloudflare_zone_pageviews_total Pageviews per zone
# HELP cloudflare_zone_requests_cached Number of cached requests for zone
# HELP cloudflare_zone_requests_content_type Number of request for zone per content type
# HELP cloudflare_zone_requests_country Number of request for zone per country
# HELP cloudflare_zone_requests_origin_status_country_host Count of not cached requests for zone per origin HTTP status per country per host
# HELP cloudflare_zone_requests_ssl_encrypted Number of encrypted requests for zone
# HELP cloudflare_zone_requests_status Number of request for zone per HTTP status
# HELP cloudflare_zone_requests_status_country_host Count of requests for zone per edge HTTP status per country per host
# HELP cloudflare_zone_requests_browser_map_page_views_count Number of successful requests for HTML pages per zone
# HELP cloudflare_zone_requests_total Number of requests for zone
# HELP cloudflare_zone_threats_country Threats per zone per country
# HELP cloudflare_zone_threats_total Threats per zone
# HELP cloudflare_zone_uniques_total Uniques per zone
# HELP cloudflare_zone_pool_health_status Reports the health of a pool, 1 for healthy, 0 for unhealthy
# HELP cloudflare_zone_pool_requests_total Requests per pool
```

## See Also
- [api token](../../observability/metrics/api-token.md) <!-- rel:strong -->
- [grafana dashboard](../../observability/metrics/grafana-dashboard.md) <!-- rel:strong -->
- [description](../../observability/metrics/description.md) <!-- rel:strong -->
- [build](../../build/build.md) <!-- rel:strong -->
- [contributing and reporting issues](../../code-conventions/git/contributing-and-reporting-issues.md) <!-- rel:strong -->
