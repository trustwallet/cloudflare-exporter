---
title: "Data Flow: fetchMetrics and the Analytics Pipeline"
category: architecture
tags: [data-flow, fetchMetrics, analytics, concurrency, zones, workers]
confidence: high
source: main.go, cloudflare.go, prometheus.go
updated: 2026-07-22
---

# Data Flow: fetchMetrics and the Analytics Pipeline

## Overview

`fetchMetrics` (main.go:103) is the central orchestration function — the god-node with
fan-out 10. It runs on a 60-second ticker (not the `SCRAPE_DELAY` env var, which is defined
but unused in the current code as a config option; the ticker is hardcoded at 60s) and drives
the entire scrape cycle for all Cloudflare analytics.

## Execution Flow

```
main()
  └── goroutine: every 60s
        └── fetchMetrics()
              ├── fetchZones()              → all zones via Cloudflare REST API
              ├── fetchAccounts()           → all accounts via Cloudflare REST API
              ├── filterZones()/filterExcludedZones() → apply CF_ZONES / CF_EXCLUDE_ZONES
              │
              ├── for each account:
              │     goroutine: fetchWorkerAnalytics(account, &wg)
              │           └── fetchWorkerTotals(accountID)  → GraphQL
              │
              └── for batches of zones (≤ cfgBatchSize, default 10):
                    goroutine: fetchZoneAnalytics(batch, &wg)
                    │     ├── filterNonFreePlanZones()
                    │     ├── fetchZoneTotals(zoneIDs)      → GraphQL
                    │     └── addHTTPGroups / addFirewallGroups / addHealthCheckGroups / addHTTPAdaptiveGroups
                    │
                    goroutine: fetchZoneColocationAnalytics(batch, &wg)
                    │     ├── filterNonFreePlanZones()
                    │     ├── fetchColoTotals(zoneIDs)      → GraphQL
                    │     └── sets colo-scoped Prometheus gauges
                    │
                    goroutine: fetchLoadBalancerAnalytics(batch, &wg)
                          ├── filterNonFreePlanZones()
                          ├── fetchLoadBalancerTotals(zoneIDs) → GraphQL
                          └── addLoadBalancingRequestsAdaptive / addLoadBalancingRequestsAdaptiveGroups
```

## Key Invariants

- **Batching is rate-limit mitigation**: Cloudflare's GraphQL API accepts at most 10 zones per
  query. `cfgBatchSize` (env: `CF_BATCH_SIZE`, default 10, max 10) controls this.  Requests for
  zone analytics, colocation analytics, and load-balancer analytics are fanned out in goroutines
  per batch, synchronized by a single `sync.WaitGroup`.
- **Metric values are NOT accumulated** — each scrape cycle calls `Set()` on all gauges, so
  Prometheus sees the current window snapshot, not a running total.
- **Zone selection**: `CF_ZONES` is an allowlist (by zone ID); `CF_EXCLUDE_ZONES` is a denylist.
  If neither is set, all zones are scraped. The old `ZONE_<NAME>` env-var pattern still works
  but is deprecated (`main.go:38-43`).
- **Free tier filtering**: `filterNonFreePlanZones` (`cloudflare.go:602`) strips zones with ID
  `0feeeeeeeeeeeeeeeeeeeeeeeeeeeeee` (the synthetic free-plan zone ID) when `CF_FREE_TIER=true`
  is not set — i.e., by default paid-plan analytics are scraped. When `free_tier=true`, the
  filter inverts.

## Cross-Domain Bridges

`fetchMetrics` is the sole bridge between three domains:
- **main.go → cloudflare.go**: zone and account discovery (`fetchZones`, `fetchAccounts`)
- **main.go → prometheus.go**: analytics fan-out (`fetchZoneAnalytics`, etc.)
- **prometheus.go → cloudflare.go**: each analytics function calls the matching GraphQL fetch
  function (`fetchZoneTotals`, `fetchColoTotals`, `fetchWorkerTotals`, `fetchLoadBalancerTotals`)

The bridge pattern means `prometheus.go` is both a metrics registry AND an analytics
orchestrator for a given data type — it calls into `cloudflare.go` to fetch raw data, then
immediately converts it to Prometheus observations. There is no intermediate data layer.

## See Also
- [cloudflare api integration](../security/cloudflare-api-integration.md) <!-- rel:strong -->
- [metrics catalog](../observability/metrics-catalog.md) <!-- rel:strong -->
- [configuration](../observability/metrics/configuration.md) <!-- rel:strong -->
- [metrics denylist](../patterns/metrics-denylist.md) <!-- rel:related -->
- [api token](../observability/metrics/api-token.md) <!-- rel:weak -->
