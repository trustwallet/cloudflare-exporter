---
title: "Cloudflare API Integration"
category: security
tags: [cloudflare-api, authentication, graphql, rate-limiting, api-token]
confidence: high
source: cloudflare.go, main.go
updated: 2026-07-22
---

# Cloudflare API Integration

## Authentication Modes

Two mutually-exclusive auth modes are supported (cloudflare.go, main.go:145):

| Mode | Env Vars | Header Set |
|------|----------|-----------|
| API Token (preferred) | `CF_API_TOKEN` | `Authorization: Bearer <token>` |
| Key + Email (legacy) | `CF_API_KEY` + `CF_API_EMAIL` | `X-AUTH-EMAIL` + `X-AUTH-KEY` |

Startup fails with a fatal log if neither mode is configured (`main.go:145`).

The API token is the recommended option as it supports granular scopes. The key+email
combination grants full account access and should be avoided for new deployments.

## API Surface Used

| Function | API Type | Endpoint/Dataset | Data |
|---|---|---|---|
| `fetchZones()` | REST (cloudflare-go SDK) | `GET /zones` | List of all zones |
| `fetchAccounts()` | REST (cloudflare-go SDK) | `GET /accounts` | List of all accounts |
| `fetchZoneTotals()` | GraphQL | `httpRequests1mGroups`, `firewallEventsAdaptiveGroups`, `healthCheckEventsAdaptiveGroups` | HTTP, firewall, health metrics |
| `fetchColoTotals()` | GraphQL | `httpRequestsAdaptiveGroups` (by colo) | Per-PoP request bytes + visits |
| `fetchWorkerTotals()` | GraphQL | `workersInvocationsAdaptive` | Worker invocations + CPU/duration quantiles |
| `fetchLoadBalancerTotals()` | GraphQL | `loadBalancingRequestsAdaptive`, `loadBalancingRequestsAdaptiveGroups` | Pool health + routing |

GraphQL endpoint: `https://api.cloudflare.com/client/v4/graphql/`
GraphQL client library: `github.com/machinebox/graphql`

## Rate Limit Mitigation

Cloudflare's Analytics GraphQL API accepts at most 10 zones per query. The exporter
respects this with `cfgBatchSize` (default 10, max 10, env: `CF_BATCH_SIZE`). Zones are
sliced into batches of this size and queried in parallel goroutines. Setting `CF_BATCH_SIZE`
lower reduces per-request payload at the cost of more API calls (useful if hitting
query-complexity limits for zones with many domains/hostnames).

## Free Tier Handling

`CF_FREE_TIER=true` restricts scraping to the synthetic free zone ID
`0feeeeeeeeeeeeeeeeeeeeeeeeeeeeee` (`cloudflare.go:602`). When false (default),
`filterNonFreePlanZones` strips out that placeholder ID so only real paid-plan zones are
scraped. This prevents noise from the free-tier zone in paid deployments.

## See Also
- [domains](../architecture/domains.md) <!-- rel:strong -->
- [data flow](../architecture/data-flow.md) <!-- rel:strong -->
- [configuration](../observability/metrics/configuration.md) <!-- rel:strong -->
- [api token](../observability/metrics/api-token.md) <!-- rel:related -->
- [metrics catalog](../observability/metrics-catalog.md) <!-- rel:related -->
