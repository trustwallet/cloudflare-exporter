# Call Graph

<!-- sdd-knowledge-generated -->

> Deterministic call graph extracted via tree-sitter (no LLM). Direct calls are resolved by lexical scope + imports; **dynamic dispatch and ambiguous name matches are withheld** rather than guessed. The `## Calls` table is `EXTRACTED` (a single resolved target). Member calls (`obj.method()`) whose method name resolves to exactly one definition repo-wide are recovered separately under `## Inferred calls` (`INFERRED` — receiver type unverified, but a single plausible target).

## Calls

| Caller | Callee | Example args | Sites |
|--------|--------|--------------|-------|
| `fetchMetrics` | `fetchAccounts(): []cloudflare.Account` | `()` | 1 |
| `fetchMetrics` | `fetchZones(): []cloudflare.Zone` | `()` | 1 |
| `fetchMetrics` | `filterExcludedZones(all: []cloudflare.Zone, exclude: []string): []cloudflare.Zone` | `(…, …)` | 1 |
| `fetchMetrics` | `filterZones(all: []cloudflare.Zone, target: []string): []cloudflare.Zone` | `(zones, …)` | 1 |
| `fetchMetrics` | `getExcludedZones(): []string` | `()` | 1 |
| `fetchMetrics` | `getTargetZones(): []string` | `()` | 1 |
| `fetchMetrics` | `fetchLoadBalancerAnalytics(zones: []cloudflare.Zone, wg: *sync.WaitGroup)` | `(targetZones, …)` | 1 |
| `fetchMetrics` | `fetchWorkerAnalytics(account: cloudflare.Account, wg: *sync.WaitGroup)` | `(a, …)` | 1 |
| `fetchMetrics` | `fetchZoneAnalytics(zones: []cloudflare.Zone, wg: *sync.WaitGroup)` | `(targetZones, …)` | 1 |
| `fetchMetrics` | `fetchZoneColocationAnalytics(zones: []cloudflare.Zone, wg: *sync.WaitGroup)` | `(targetZones, …)` | 1 |
| `filterExcludedZones` | `contains(s: []string, e: string): bool` | `(exclude, z.ID)` | 1 |
| `main` | `fetchMetrics()` | `()` | 1 |
| `main` | `buildDeniedMetricsSet(metricsDenylist: []string): (MetricsSet, error)` | `(metricsDenylist)` | 1 |
| `main` | `mustRegisterMetrics(deniedMetrics: MetricsSet)` | `(deniedMetricsSet)` | 1 |
| `buildDeniedMetricsSet` | `buildAllMetricsSet(): MetricsSet` | `()` | 1 |
| `buildDeniedMetricsSet` | `MetricName` | `(metric)` | 2 |
| `fetchLoadBalancerAnalytics` | `extractZoneIDs(zones: []cloudflare.Zone): []string` | `(…)` | 1 |
| `fetchLoadBalancerAnalytics` | `fetchLoadBalancerTotals(zoneIDs: []string): (*cloudflareResponseLb, error)` | `(zoneIDs)` | 1 |
| `fetchLoadBalancerAnalytics` | `filterNonFreePlanZones(zones: []cloudflare.Zone): (filteredZones []cloudflare.Zone)` | `(zones)` | 1 |
| `fetchLoadBalancerAnalytics` | `findZoneName(zones: []cloudflare.Zone, ID: string): string` | `(zones, lb.ZoneTag)` | 1 |
| `fetchLoadBalancerAnalytics` | `addLoadBalancingRequestsAdaptive(z: *lbResp, name: string)` | `(…, name)` | 1 |
| `fetchLoadBalancerAnalytics` | `addLoadBalancingRequestsAdaptiveGroups(z: *lbResp, name: string)` | `(…, name)` | 1 |
| `fetchWorkerAnalytics` | `fetchWorkerTotals(accountID: string): (*cloudflareResponseAccts, error)` | `(account.ID)` | 1 |
| `fetchZoneAnalytics` | `extractZoneIDs(zones: []cloudflare.Zone): []string` | `(…)` | 1 |
| `fetchZoneAnalytics` | `fetchZoneTotals(zoneIDs: []string): (*cloudflareResponse, error)` | `(zoneIDs)` | 1 |
| `fetchZoneAnalytics` | `filterNonFreePlanZones(zones: []cloudflare.Zone): (filteredZones []cloudflare.Zone)` | `(zones)` | 1 |
| `fetchZoneAnalytics` | `findZoneName(zones: []cloudflare.Zone, ID: string): string` | `(zones, z.ZoneTag)` | 1 |
| `fetchZoneAnalytics` | `addFirewallGroups(z: *zoneResp, name: string)` | `(…, name)` | 1 |
| `fetchZoneAnalytics` | `addHealthCheckGroups(z: *zoneResp, name: string)` | `(…, name)` | 1 |
| `fetchZoneAnalytics` | `addHTTPAdaptiveGroups(z: *zoneResp, name: string)` | `(…, name)` | 1 |
| `fetchZoneAnalytics` | `addHTTPGroups(z: *zoneResp, name: string)` | `(…, name)` | 1 |
| `fetchZoneColocationAnalytics` | `extractZoneIDs(zones: []cloudflare.Zone): []string` | `(…)` | 1 |
| `fetchZoneColocationAnalytics` | `fetchColoTotals(zoneIDs: []string): (*cloudflareResponseColo, error)` | `(zoneIDs)` | 1 |
| `fetchZoneColocationAnalytics` | `filterNonFreePlanZones(zones: []cloudflare.Zone): (filteredZones []cloudflare.Zone)` | `(zones)` | 1 |
| `fetchZoneColocationAnalytics` | `findZoneName(zones: []cloudflare.Zone, ID: string): string` | `(zones, z.ZoneTag)` | 1 |

## Callers (reverse)

| Symbol | Called by |
|--------|-----------|
| `addFirewallGroups` | `fetchZoneAnalytics` |
| `addHealthCheckGroups` | `fetchZoneAnalytics` |
| `addHTTPAdaptiveGroups` | `fetchZoneAnalytics` |
| `addHTTPGroups` | `fetchZoneAnalytics` |
| `addLoadBalancingRequestsAdaptive` | `fetchLoadBalancerAnalytics` |
| `addLoadBalancingRequestsAdaptiveGroups` | `fetchLoadBalancerAnalytics` |
| `buildAllMetricsSet` | `buildDeniedMetricsSet` |
| `buildDeniedMetricsSet` | `main` |
| `contains` | `filterExcludedZones` |
| `extractZoneIDs` | `fetchLoadBalancerAnalytics`, `fetchZoneAnalytics`, `fetchZoneColocationAnalytics` |
| `fetchAccounts` | `fetchMetrics` |
| `fetchColoTotals` | `fetchZoneColocationAnalytics` |
| `fetchLoadBalancerAnalytics` | `fetchMetrics` |
| `fetchLoadBalancerTotals` | `fetchLoadBalancerAnalytics` |
| `fetchMetrics` | `main` |
| `fetchWorkerAnalytics` | `fetchMetrics` |
| `fetchWorkerTotals` | `fetchWorkerAnalytics` |
| `fetchZoneAnalytics` | `fetchMetrics` |
| `fetchZoneColocationAnalytics` | `fetchMetrics` |
| `fetchZones` | `fetchMetrics` |
| `fetchZoneTotals` | `fetchZoneAnalytics` |
| `filterExcludedZones` | `fetchMetrics` |
| `filterNonFreePlanZones` | `fetchLoadBalancerAnalytics`, `fetchZoneAnalytics`, `fetchZoneColocationAnalytics` |
| `filterZones` | `fetchMetrics` |
| `findZoneName` | `fetchLoadBalancerAnalytics`, `fetchZoneAnalytics`, `fetchZoneColocationAnalytics` |
| `getExcludedZones` | `fetchMetrics` |
| `getTargetZones` | `fetchMetrics` |
| `MetricName` | `buildDeniedMetricsSet` |
| `mustRegisterMetrics` | `main` |

## Inferred calls (member, unique name)

> `INFERRED` (confidence 0.9): `obj.method()` where `method` resolves to exactly one definition repo-wide. Receiver type is not resolved; common method names (init/update/onCreate) remain withheld as ambiguous. Use SCIP (`--scip`) for type-resolved member calls at full certainty.

| Caller | Callee | Example args | Sites |
|--------|--------|--------------|-------|
| `fetchColoTotals` | `Add(mn: MetricName)` | `(…)` | 2 |
| `fetchLoadBalancerTotals` | `Add(mn: MetricName)` | `(…)` | 2 |
| `fetchWorkerTotals` | `Add(mn: MetricName)` | `(…)` | 2 |
| `fetchZoneTotals` | `Add(mn: MetricName)` | `(…)` | 2 |
| `addFirewallGroups` | `Add(mn: MetricName)` | `(…)` | 1 |
| `addHealthCheckGroups` | `Add(mn: MetricName)` | `(…)` | 1 |
| `addHTTPAdaptiveGroups` | `Add(mn: MetricName)` | `(…)` | 2 |
| `addHTTPGroups` | `Add(mn: MetricName)` | `(…)` | 17 |
| `addLoadBalancingRequestsAdaptiveGroups` | `Add(mn: MetricName)` | `(…)` | 1 |
| `buildAllMetricsSet` | `Add(mn: MetricName)` | `(zoneRequestTotalMetricName)` | 30 |
| `buildDeniedMetricsSet` | `Add(mn: MetricName)` | `(…)` | 1 |
| `buildDeniedMetricsSet` | `Has(mn: MetricName): bool` | `(…)` | 1 |
| `fetchLoadBalancerAnalytics` | `Add(mn: MetricName)` | `(1)` | 1 |
| `fetchWorkerAnalytics` | `Add(mn: MetricName)` | `(1)` | 3 |
| `fetchZoneAnalytics` | `Add(mn: MetricName)` | `(1)` | 1 |
| `fetchZoneColocationAnalytics` | `Add(mn: MetricName)` | `(1)` | 4 |
| `mustRegisterMetrics` | `Has(mn: MetricName): bool` | `(zoneRequestTotalMetricName)` | 30 |

## Graph analytics

- Call edges: **35** across **43** symbols
- Reachable from entry points (exported symbols + routes): **6**
- Dependency cycles: **0**
- Cross-domain bridges: **21**
- Dead-code candidates (non-exported, zero callers, unreachable): **9**

### Most-coupled symbols (god-node ranking)

| Symbol | Fan-in | Fan-out | Degree |
|--------|--------|---------|--------|
| `fetchMetrics` | 1 | 10 | 11 |
| `fetchZoneAnalytics` | 1 | 8 | 9 |
| `fetchLoadBalancerAnalytics` | 1 | 6 | 7 |
| `fetchZoneColocationAnalytics` | 1 | 4 | 5 |
| `extractZoneIDs` | 3 | 0 | 3 |
| `filterNonFreePlanZones` | 3 | 0 | 3 |
| `findZoneName` | 3 | 0 | 3 |
| `main` | 0 | 3 | 3 |
| `buildDeniedMetricsSet` | 1 | 2 | 3 |
| `filterExcludedZones` | 1 | 1 | 2 |
| `fetchWorkerAnalytics` | 1 | 1 | 2 |
| `fetchAccounts` | 1 | 0 | 1 |
| `fetchColoTotals` | 1 | 0 | 1 |
| `fetchLoadBalancerTotals` | 1 | 0 | 1 |
| `fetchWorkerTotals` | 1 | 0 | 1 |

### Cross-domain bridges

> Calls that cross a domain boundary — the integration points between subsystems. A sole link between two domains is the connection you'd least expect.

| From (domain) | To (domain) | Domain links |
|------|------|-----|
| `fetchMetrics` (main.go) | `fetchAccounts` (cloudflare.go) | 2 |
| `fetchMetrics` (main.go) | `fetchZones` (cloudflare.go) | 2 |
| `fetchMetrics` (main.go) | `fetchLoadBalancerAnalytics` (prometheus.go) | 6 |
| `fetchMetrics` (main.go) | `fetchWorkerAnalytics` (prometheus.go) | 6 |
| `fetchMetrics` (main.go) | `fetchZoneAnalytics` (prometheus.go) | 6 |
| `fetchMetrics` (main.go) | `fetchZoneColocationAnalytics` (prometheus.go) | 6 |
| `main` (main.go) | `buildDeniedMetricsSet` (prometheus.go) | 6 |
| `main` (main.go) | `mustRegisterMetrics` (prometheus.go) | 6 |
| `fetchLoadBalancerAnalytics` (prometheus.go) | `extractZoneIDs` (cloudflare.go) | 13 |
| `fetchLoadBalancerAnalytics` (prometheus.go) | `fetchLoadBalancerTotals` (cloudflare.go) | 13 |
| `fetchLoadBalancerAnalytics` (prometheus.go) | `filterNonFreePlanZones` (cloudflare.go) | 13 |
| `fetchLoadBalancerAnalytics` (prometheus.go) | `findZoneName` (cloudflare.go) | 13 |
| `fetchWorkerAnalytics` (prometheus.go) | `fetchWorkerTotals` (cloudflare.go) | 13 |
| `fetchZoneAnalytics` (prometheus.go) | `extractZoneIDs` (cloudflare.go) | 13 |
| `fetchZoneAnalytics` (prometheus.go) | `fetchZoneTotals` (cloudflare.go) | 13 |
| `fetchZoneAnalytics` (prometheus.go) | `filterNonFreePlanZones` (cloudflare.go) | 13 |
| `fetchZoneAnalytics` (prometheus.go) | `findZoneName` (cloudflare.go) | 13 |
| `fetchZoneColocationAnalytics` (prometheus.go) | `extractZoneIDs` (cloudflare.go) | 13 |
| `fetchZoneColocationAnalytics` (prometheus.go) | `fetchColoTotals` (cloudflare.go) | 13 |
| `fetchZoneColocationAnalytics` (prometheus.go) | `filterNonFreePlanZones` (cloudflare.go) | 13 |
| `fetchZoneColocationAnalytics` (prometheus.go) | `findZoneName` (cloudflare.go) | 13 |

### Dead-code candidates

> Non-exported symbols with no resolved callers, unreachable from any entry point. Static analysis cannot see dynamic dispatch — verify before removing.

- `accountResp` (`cloudflare.go`)
- `cloudflareResponse` (`cloudflare.go`)
- `cloudflareResponseAccts` (`cloudflare.go`)
- `cloudflareResponseColo` (`cloudflare.go`)
- `cloudflareResponseLb` (`cloudflare.go`)
- `lbResp` (`cloudflare.go`)
- `zoneResp` (`cloudflare.go`)
- `zoneRespColo` (`cloudflare.go`)
- `main` (`main.go`)

