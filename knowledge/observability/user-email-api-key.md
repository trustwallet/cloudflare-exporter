---
category: observability
confidence: low
documentType: reference
scope: org
contentHash: 0634bd4c4558
tags: [dashboard]
source: README.md
verified: 2026-07-22
---

## User email + API key

To authenticate with user email + API key, use the `Global API Key` from the Cloudflare dashboard.
Beware that this key authenticates with write access to every Cloudflare resource.

To authenticate this way, set both `CF_API_KEY` and `CF_API_EMAIL`.

## See Also
- [run](../ci/workflows/run.md) <!-- rel:strong -->
- [authentication](../security/auth/authentication.md) <!-- rel:related -->
- [build](../build/build.md) <!-- rel:related -->
- [cloudflare api integration](../security/cloudflare-api-integration.md) <!-- rel:strong -->
- [domains](../architecture/domains.md) <!-- rel:related -->
