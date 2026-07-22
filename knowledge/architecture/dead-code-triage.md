---
title: "Dead-Code Triage: GraphQL Response Structs"
category: architecture
tags: [dead-code, structs, json-unmarshaling, false-positive]
confidence: high
source: cloudflare.go
updated: 2026-07-22
---

# Dead-Code Triage: GraphQL Response Structs

## Static Analysis Result

The relation graph marks the following symbols as dead-code candidates
(non-exported, zero resolved callers, unreachable from entry points):

- `accountResp` (cloudflare.go:40)
- `cloudflareResponse` (cloudflare.go:16)
- `cloudflareResponseAccts` (cloudflare.go:22)
- `cloudflareResponseColo` (cloudflare.go:28)
- `cloudflareResponseLb` (cloudflare.go:34)
- `lbResp` (cloudflare.go:182)
- `zoneResp` (cloudflare.go:86)
- `zoneRespColo` (cloudflare.go:66)

## Verdict: **All False Positives — Do Not Delete**

These are JSON response structs populated exclusively via `json.Unmarshal` in the
`fetch*Totals` functions. Static analysis cannot see the dynamic unmarshaling path:

```go
// cloudflare.go — typical pattern
var r cloudflareResponse
err := graphqlClient.Run(ctx, req, &r)   // json.Unmarshal fills `r` at runtime
```

The structs are live data contracts with the Cloudflare GraphQL API. Deleting them would
break all analytics at runtime. The tree-sitter extractor withholds this dynamic dispatch
edge correctly — the "dead" label is expected, not a real problem.

## `main` (main.go:132) Also Flagged

The Go entrypoint `main()` has fan-in 0 because nothing in the repo calls it explicitly —
the runtime is the caller. This is universally expected for any Go `main` function and is
safe to ignore.

## See Also
- [cloudflare api integration](../security/cloudflare-api-integration.md) <!-- rel:strong -->
- [metrics catalog](../observability/metrics-catalog.md) <!-- rel:strong -->
- [configuration](../observability/metrics/configuration.md) <!-- rel:strong -->
- [api token](../observability/metrics/api-token.md) <!-- rel:related -->
- [learnings](../learnings.md) <!-- rel:related -->
