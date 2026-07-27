---
title: "Observability: Exported Metrics Catalog"
category: observability
tags: [prometheus, metrics, cloudflare, zones, workers, load-balancer, colo]
confidence: high
source: prometheus.go, ci/workflows/list-of-available-metrics.md
updated: 2026-07-22
---

# Observability: Exported Metrics Catalog

## Service Endpoints

| Endpoint | Handler | Purpose |
|---|---|---|
| `GET /metrics` (default, configurable via `METRICS_PATH`) | `promhttp.Handler()` | Prometheus scrape endpoint |
| `GET /health` | `nelkinda/health-go` | Health check — returns 200 OK |

The listen address is configured via `LISTEN` env var (default `:8080`).

## Metric Domains

All 30 metrics are defined in `prometheus.go` as package-level `CounterVec` / `GaugeVec`
variables. They fall into four domains matching the four GraphQL datasources:

### Zone HTTP Metrics (from `fetchZoneAnalytics`)

Covers total requests, cached requests, bandwidth, browser map, HTTP versions, SSL protocols,
content-type breakdown, country breakdown, response status, threat pathing, and IP class.

Labels typically include: `zone` (zone name), `host` (`clientRequestHTTPHost`), `country`
(`clientCountryName`), `status` (HTTP response code).

### Colocation Metrics (from `fetchZoneColocationAnalytics`)

Per-Cloudflare-PoP request bytes and visit counts.

Labels: `zone`, `colo_code` (IATA airport code), `host`.

### Worker Metrics (from `fetchWorkerAnalytics`)

Worker invocation counts, error counts, duration, and CPU-time quantiles at P50/P75/P99/P999.

Labels: `script_name`, `status`.

### Load Balancer Metrics (from `fetchLoadBalancerAnalytics`)

Request routing counts grouped by pool, origin health status, RTT, steering policy, and
session-affinity status.

Labels: `lb_name`, `pool_name`, `origin_name`, `region`, `steering_policy`, `healthy`.

## Scrape Timing

The scrape cycle runs every **60 seconds** (hardcoded in `main.go:167`). The `SCRAPE_DELAY`
config option is parsed but currently unused — the ticker is not driven by it. All gauges
are overwritten on each cycle (no accumulation), so the metrics reflect the latest
Cloudflare analytics window.

## Metric Denylist

See [patterns/metrics-denylist.md](../patterns/metrics-denylist.md) for how to suppress
individual metrics at startup via `METRICS_DENYLIST`.

## See Also
- [domains](../architecture/domains.md) <!-- rel:strong -->
- [data flow](../architecture/data-flow.md) <!-- rel:strong -->
- [cloudflare api integration](../security/cloudflare-api-integration.md) <!-- rel:related -->
- [metrics denylist](../patterns/metrics-denylist.md) <!-- rel:related -->
- [models](../architecture/data/models.md) <!-- rel:related -->
