# Data Models & Schemas

<!-- sdd-knowledge-generated -->

> Field-level shape of data models extracted via tree-sitter: TS interfaces / type aliases, Zod `z.object` schemas, Go/Rust/Swift structs, Kotlin data classes, and Python dataclasses. Scoped to domain data — UI views/props, view-models, design tokens (theme/style/colors), and constant/identifier namespaces are excluded. Deterministic, no LLM.

## accountResp

_struct · `cloudflare.go`:40_

| Field | Type | Optional |
|-------|------|----------|
| `WorkersInvocationsAdaptive` | `[]struct { Dimensions struct { ScriptName string `json:"scriptName"` Status string `json:"status"` } Sum struct { Requests uint64 `json:"requests"` Errors uint64 `json:"errors"` Duration float64 `json:"duration"` } `json:"sum"` Quantiles struct { CPUTimeP50 float32 `json:"cpuTimeP50"` CPUTimeP75 float32 `json:"cpuTimeP75"` CPUTimeP99 float32 `json:"cpuTimeP99"` CPUTimeP999 float32 `json:"cpuTimeP999"` DurationP50 float32 `json:"durationP50"` DurationP75 float32 `json:"durationP75"` DurationP99 float32 `json:"durationP99"` DurationP999 float32 `json:"durationP999"` } `json:"quantiles"` }` | no |

## cloudflareResponse

_struct · `cloudflare.go`:16_

| Field | Type | Optional |
|-------|------|----------|
| `Viewer` | `struct { Zones []zoneResp `json:"zones"` }` | no |

## cloudflareResponseAccts

_struct · `cloudflare.go`:22_

| Field | Type | Optional |
|-------|------|----------|
| `Viewer` | `struct { Accounts []accountResp `json:"accounts"` }` | no |

## cloudflareResponseColo

_struct · `cloudflare.go`:28_

| Field | Type | Optional |
|-------|------|----------|
| `Viewer` | `struct { Zones []zoneRespColo `json:"zones"` }` | no |

## cloudflareResponseLb

_struct · `cloudflare.go`:34_

| Field | Type | Optional |
|-------|------|----------|
| `Viewer` | `struct { Zones []lbResp `json:"zones"` }` | no |

## lbResp

_struct · `cloudflare.go`:182_

| Field | Type | Optional |
|-------|------|----------|
| `LoadBalancingRequestsAdaptiveGroups` | `[]struct { Count uint64 `json:"count"` Dimensions struct { LbName string `json:"lbName"` Proxied uint8 `json:"proxied"` Region string `json:"region"` SelectedOriginName string `json:"selectedOriginName"` SelectedPoolAvgRttMs uint64 `json:"selectedPoolAvgRttMs"` SelectedPoolHealthy uint8 `json:"selectedPoolHealthy"` SelectedPoolName string `json:"selectedPoolName"` SteeringPolicy string `json:"steeringPolicy"` } `json:"dimensions"` }` | no |
| `LoadBalancingRequestsAdaptive` | `[]struct { LbName string `json:"lbName"` Proxied uint8 `json:"proxied"` Region string `json:"region"` SelectedPoolHealthy uint8 `json:"selectedPoolHealthy"` SelectedPoolID string `json:"selectedPoolID"` SelectedPoolName string `json:"selectedPoolName"` SessionAffinityStatus string `json:"sessionAffinityStatus"` SteeringPolicy string `json:"steeringPolicy"` SelectedPoolAvgRttMs uint64 `json:"selectedPoolAvgRttMs"` Pools []struct { AvgRttMs uint64 `json:"avgRttMs"` Healthy uint8 `json:"healthy"` ID string `json:"id"` PoolName string `json:"poolName"` } `json:"pools"` Origins []struct { OriginName string `json:"originName"` Health uint8 `json:"health"` IPv4 string `json:"ipv4"` Selected uint8 `json:"selected"` } `json:"origins"` }` | no |
| `ZoneTag` | `string` | no |

## zoneResp

_struct · `cloudflare.go`:86_

| Field | Type | Optional |
|-------|------|----------|
| `HTTP1mGroups` | `[]struct { Dimensions struct { Datetime string `json:"datetime"` } `json:"dimensions"` Unique struct { Uniques uint64 `json:"uniques"` } `json:"uniq"` Sum struct { Bytes uint64 `json:"bytes"` CachedBytes uint64 `json:"cachedBytes"` CachedRequests uint64 `json:"cachedRequests"` Requests uint64 `json:"requests"` BrowserMap []struct { PageViews uint64 `json:"pageViews"` UaBrowserFamily string `json:"uaBrowserFamily"` } `json:"browserMap"` ClientHTTPVersion []struct { Protocol string `json:"clientHTTPProtocol"` Requests uint64 `json:"requests"` } `json:"clientHTTPVersionMap"` ClientSSL []struct { Protocol string `json:"clientSSLProtocol"` } `json:"clientSSLMap"` ContentType []struct { Bytes uint64 `json:"bytes"` Requests uint64 `json:"requests"` EdgeResponseContentType string `json:"edgeResponseContentTypeName"` } `json:"contentTypeMap"` Country []struct { Bytes uint64 `json:"bytes"` ClientCountryName string `json:"clientCountryName"` Requests uint64 `json:"requests"` Threats uint64 `json:"threats"` } `json:"countryMap"` EncryptedBytes uint64 `json:"encryptedBytes"` EncryptedRequests uint64 `json:"encryptedRequests"` IPClass []struct { Type string `json:"ipType"` Requests uint64 `json:"requests"` } `json:"ipClassMap"` PageViews uint64 `json:"pageViews"` ResponseStatus []struct { EdgeResponseStatus int `json:"edgeResponseStatus"` Requests uint64 `json:"requests"` } `json:"responseStatusMap"` ThreatPathing []struct { Name string `json:"threatPathingName"` Requests uint64 `json:"requests"` } `json:"threatPathingMap"` Threats uint64 `json:"threats"` } `json:"sum"` }` | no |
| `FirewallEventsAdaptiveGroups` | `[]struct { Count uint64 `json:"count"` Dimensions struct { Action string `json:"action"` Source string `json:"source"` ClientCountryName string `json:"clientCountryName"` ClientRequestHTTPHost string `json:"clientRequestHTTPHost"` } `json:"dimensions"` }` | no |
| `HTTPRequestsAdaptiveGroups` | `[]struct { Count uint64 `json:"count"` Dimensions struct { OriginResponseStatus uint16 `json:"originResponseStatus"` ClientCountryName string `json:"clientCountryName"` ClientRequestHTTPHost string `json:"clientRequestHTTPHost"` } `json:"dimensions"` }` | no |
| `HTTPRequestsEdgeCountryHost` | `[]struct { Count uint64 `json:"count"` Dimensions struct { EdgeResponseStatus uint16 `json:"edgeResponseStatus"` ClientCountryName string `json:"clientCountryName"` ClientRequestHTTPHost string `json:"clientRequestHTTPHost"` } `json:"dimensions"` }` | no |
| `HealthCheckEventsAdaptiveGroups` | `[]struct { Count uint64 `json:"count"` Dimensions struct { HealthStatus string `json:"healthStatus"` OriginIP string `json:"originIP"` FailureReason string `json:"failureReason"` Region string `json:"region"` Fqdn string `json:"fqdn"` } `json:"dimensions"` }` | no |
| `ZoneTag` | `string` | no |

## zoneRespColo

_struct · `cloudflare.go`:66_

| Field | Type | Optional |
|-------|------|----------|
| `ColoGroups` | `[]struct { Dimensions struct { Datetime string `json:"datetime"` ColoCode string `json:"coloCode"` Host string `json:"clientRequestHTTPHost"` } `json:"dimensions"` Count uint64 `json:"count"` Sum struct { EdgeResponseBytes uint64 `json:"edgeResponseBytes"` Visits uint64 `json:"visits"` } `json:"sum"` Avg struct { SampleInterval float64 `json:"sampleInterval"` } `json:"avg"` }` | no |
| `ZoneTag` | `string` | no |

