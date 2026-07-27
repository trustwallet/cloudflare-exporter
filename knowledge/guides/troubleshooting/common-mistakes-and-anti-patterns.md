---
category: guides
subcategory: troubleshooting
confidence: low
documentType: how-to
scope: org
contentHash: efd86bbf598d
tags: [anti-pattern, troubleshooting, faq]
source: (synthesized)
verified: 2026-07-22
synthetic: synthesized-faq
---

## Common Mistakes and Anti-Patterns

<!-- sdd-knowledge-synthesized -->

> Auto-generated from anti-patterns found across the knowledge base.

### Architecture

**From [Dead-Code Triage: GraphQL Response Structs](../../architecture/dead-code-triage-graphql-response-structs.md):**
These are JSON response structs populated exclusively via `json.Unmarshal` in the

### Code-conventions

**From [Anti-Patterns (failed approaches)](../../code-conventions/anti-patterns-failed-approaches.md):**
<!-- Add failed approaches here. Each anti-pattern should include:
- **type**: anti-pattern
- **discovered**: YYYY-MM-DD

### Patterns

**From [Metrics Denylist Pattern (MetricName/MetricsSet)](../../patterns/metrics-denylist-pattern-metricname-metricsset.md):**
A metric in the denylist is never registered with Prometheus, so no empty time series
appear in the scrape output.

