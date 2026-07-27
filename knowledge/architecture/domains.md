---
title: "Domain Architecture: Three-File Structure"
category: architecture
tags: [domains, main, cloudflare, prometheus, separation-of-concerns]
confidence: high
source: main.go, cloudflare.go, prometheus.go
updated: 2026-07-22
---

# Domain Architecture: Three-File Structure

This repo is intentionally minimal — the entire application is three files in `package main`.
Each file forms a logical domain with clear responsibilities.

## Domain: `main.go` — Entry Point and Orchestration

**Responsibility**: configuration, zone filtering, scrape scheduling, HTTP server.

- Parses all config from flags/env vars via `github.com/namsral/flag` (supports env vars
  automatically: `CF_API_TOKEN`, `CF_ZONES`, etc.)
- Initialises the metrics registry by calling `buildDeniedMetricsSet` + `mustRegisterMetrics`
  before launching the scrape loop
- Launches a goroutine ticker (`60s`) that calls `fetchMetrics` on each tick
- Starts the HTTP server exposing `/metrics` (promhttp) and `/health`
- Owns zone filtering logic (`filterZones`, `filterExcludedZones`, `getTargetZones`,
  `getExcludedZones`)

**Talks to**:
- `cloudflare.go`: calls `fetchZones()` and `fetchAccounts()` to discover available zones/accounts
- `prometheus.go`: calls `buildDeniedMetricsSet`, `mustRegisterMetrics`, and all
  `fetch*Analytics` functions

## Domain: `cloudflare.go` — API Client and Data Fetching

**Responsibility**: all communication with the Cloudflare API. Contains both the REST calls
(zone/account list) and four GraphQL queries.

- `fetchZones()` and `fetchAccounts()` — REST API via `github.com/cloudflare/cloudflare-go` SDK
- `fetchZoneTotals(zoneIDs)` — GraphQL: HTTP 1m request groups, firewall events, health-check events
- `fetchColoTotals(zoneIDs)` — GraphQL: per-PoP (colocation) request bytes and visit counts
- `fetchWorkerTotals(accountID)` — GraphQL: Worker invocation counts + CPU/duration quantiles
- `fetchLoadBalancerTotals(zoneIDs)` — GraphQL: load balancer pool health and routing decisions
- Auth helpers: `extractZoneIDs`, `filterNonFreePlanZones`, `findZoneName`

All GraphQL queries target `https://api.cloudflare.com/client/v4/graphql/` using
`github.com/machinebox/graphql`. Two auth modes are supported:
- **API Token** (preferred): `Authorization: Bearer <token>` (`CF_API_TOKEN`)
- **Key+Email** (legacy): `X-AUTH-EMAIL` + `X-AUTH-KEY` (`CF_API_KEY` + `CF_API_EMAIL`)

**Talks to**:
- `prometheus.go` (via `MetricsSet.Add`): adds metrics to the in-flight denylist set during
  query construction (confidence: medium — inferred from unique method name)

## Domain: `prometheus.go` — Metrics Registry and Analytics Processing

**Responsibility**: Prometheus metric definitions, denylist filtering, and analytics
conversion from Cloudflare response structs to Prometheus observations.

- Defines 30 Prometheus metrics as package-level `CounterVec`/`GaugeVec` globals
- `MetricName` type (string alias) + 30 constants, one per metric
- `MetricsSet` map type with `Has` and `Add` methods for O(1) denylist checks
- `buildAllMetricsSet()` — enumerates all 30 known metric names
- `buildDeniedMetricsSet(denylist)` — validates denylist names against the full set;
  returns error if an unknown name is supplied
- `mustRegisterMetrics(deniedMetrics)` — registers only non-denied metrics with Prometheus;
  panics on registration failure
- Four analytics-to-prometheus conversion paths (one per Cloudflare data type),
  each taking a batch of zones + WaitGroup:
  - `fetchZoneAnalytics` → `addHTTPGroups`, `addFirewallGroups`, `addHealthCheckGroups`, `addHTTPAdaptiveGroups`
  - `fetchZoneColocationAnalytics` → sets colo gauges directly
  - `fetchWorkerAnalytics` → sets worker CPU/duration quantile gauges
  - `fetchLoadBalancerAnalytics` → `addLoadBalancingRequestsAdaptive`, `addLoadBalancingRequestsAdaptiveGroups`

**Talks to**:
- `cloudflare.go`: calls all four `fetch*Totals` functions to retrieve raw GraphQL data
- `main.go`: exposes `build*MetricsSet`, `mustRegisterMetrics`, and all `fetch*Analytics` as
  the surface `main.go` invokes

## Why This Structure Works

The three-file layout is intentional for a small, focused exporter. There is no service or
repository abstraction because there is no persistent state — each scrape cycle is stateless,
all data flows straight from Cloudflare API → struct unmarshal → Prometheus `Set()`. The only
shared state is the package-level Prometheus metric variables, which Prometheus's own registry
manages as a singleton.

## See Also
- [cloudflare api integration](../security/cloudflare-api-integration.md) <!-- rel:strong -->
- [metrics catalog](../observability/metrics-catalog.md) <!-- rel:strong -->
- [metrics denylist](../patterns/metrics-denylist.md) <!-- rel:related -->
- [configuration](../observability/metrics/configuration.md) <!-- rel:related -->
- [api token](../observability/metrics/api-token.md) <!-- rel:related -->
