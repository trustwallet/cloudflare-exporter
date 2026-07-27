---
title: "Metrics Denylist Pattern (MetricName/MetricsSet)"
category: patterns
tags: [metrics, denylist, MetricName, MetricsSet, prometheus]
confidence: high
source: prometheus.go
updated: 2026-07-22
---

# Metrics Denylist Pattern (MetricName/MetricsSet)

## Purpose

Allows operators to suppress specific Prometheus metrics at startup without recompiling.
A metric in the denylist is never registered with Prometheus, so no empty time series
appear in the scrape output.

## Types

### `MetricName` (prometheus.go:13)

A string type alias. The 30 metric name constants are defined as `MetricName` constants
(e.g. `zoneRequestTotalMetricName`, `workerCPUTimeMetricName`). This prevents raw string
comparisons scattered across the codebase.

### `MetricsSet` (prometheus.go:52)

```go
type MetricsSet map[MetricName]struct{}

func (ms MetricsSet) Has(mn MetricName) bool
func (ms MetricsSet) Add(mn MetricName)
```

An O(1) set backed by a Go map. Used both to enumerate all known metrics
(`buildAllMetricsSet`) and to track which metrics are denied.

## Flow

```
startup (main.go):
  METRICS_DENYLIST="metricA,metricB"
     │
     ▼
  buildDeniedMetricsSet(["metricA","metricB"])
     ├── validates each name against buildAllMetricsSet()   // error if unknown
     └── returns MetricsSet{metricA, metricB}
     │
     ▼
  mustRegisterMetrics(deniedMetricsSet)
     └── for each of the 30 metrics:
           if !deniedMetricsSet.Has(name): prometheus.MustRegister(metric)
```

## Contract

- **Unknown metric names in the denylist cause a fatal startup error** — the validation
  in `buildDeniedMetricsSet` intentionally fails fast rather than silently ignoring
  misspellings.
- **Metric registration happens once at startup** — adding/removing from the denylist
  requires a restart.
- The denylist is configured via `METRICS_DENYLIST` env var or `--metrics_denylist` flag,
  as a comma-separated list of metric names (use the constants from `prometheus.go` as the
  authoritative name list).

## See Also
- [domains](../architecture/domains.md) <!-- rel:strong -->
- [metrics catalog](../observability/metrics-catalog.md) <!-- rel:strong -->
- [call graph](../architecture/call-graph.md) <!-- rel:strong -->
- [data flow](../architecture/data-flow.md) <!-- rel:related -->
- [configuration](../observability/metrics/configuration.md) <!-- rel:related -->
