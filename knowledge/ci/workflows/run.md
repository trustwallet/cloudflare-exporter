---
category: ci
subcategory: workflows
confidence: high
documentType: how-to
scope: org
contentHash: 042912076bf6
tags: [docker, run ]
source: README.md
verified: 2026-07-22
---

## Run

Authenticating with email + API key:
```
docker run --rm -p 8080:8080 -e CF_API_KEY=${CF_API_KEY} -e CF_API_EMAIL=${CF_API_EMAIL} ghcr.io/lablabs/cloudflare_exporter
```

API token:
```
docker run --rm -p 8080:8080 -e CF_API_TOKEN=${CF_API_TOKEN} ghcr.io/lablabs/cloudflare_exporter
```

Configure zones and listening port:
```
docker run --rm -p 8080:8081 -e CF_API_TOKEN=${CF_API_TOKEN} -e CF_ZONES=zoneid1,zoneid2,zoneid3 -e LISTEN=:8081 ghcr.io/lablabs/cloudflare_exporter
```

Disable non-free metrics:
```
docker run --rm -p 8080:8080 -e CF_API_TOKEN=${CF_API_TOKEN} -e FREE_TIER=true ghcr.io/lablabs/cloudflare_exporter
```

Access help:
```
docker run --rm -p 8080:8080 -i ghcr.io/lablabs/cloudflare_exporter --help
```

## See Also
- [configuration](../../observability/metrics/configuration.md) <!-- rel:strong -->
- [api token](../../observability/metrics/api-token.md) <!-- rel:strong -->
- [user email api key](../../observability/user-email-api-key.md) <!-- rel:strong -->
- [authentication](../../security/auth/authentication.md) <!-- rel:related -->
- [description](../../observability/metrics/description.md) <!-- rel:weak -->
