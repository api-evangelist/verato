---
name: Resolve and match a person identity
description: Add or update a source record in Verato Universal Identity and find matching identities by demographics.
api: openapi/verato-person-openapi.yml
operations:
  - post_v2_postIdentity
  - post_v2_demographicsSearch
  - post_v2_demographicsQuery
---

# Resolve and match a person identity

Use the Verato Person API to push a customer source record into the Universal
Identity Platform and to find the resolved identity that matches a set of
demographics.

## Preconditions
- Credentials: HTTP **Basic** auth over **mutual TLS**. Present your client
  certificate at the TLS layer and the Basic credentials your Verato Customer
  Success Manager provisioned for the tenant.
- All calls are `POST` with `Content-Type: application/json`; responses are JSON.

## Steps
1. **Register the source record** — call `post_v2_postIdentity` with the source
   system's native id and the person's demographics (name, address, DOB, etc.).
   Verato resolves it against the referential database and returns the identity
   grouped by source plus the linkId / vinIds.
2. **Search by demographics** — call `post_v2_demographicsSearch` with the search
   demographics and, optionally, `pageNumber` / `pageSize`, `maxSearchResults`,
   and a `matchScoreThreshold` (or `minMatchScore` / `maxMatchScore`). Ranked
   candidate matches come back in `searchResults`.
3. **Query a known identity** — when you already hold demographics for a
   specific person and want the single resolved identity, call
   `post_v2_demographicsQuery`.

## Rules
- **Error handling:** every response is HTTP 200 with a `ServiceResponse`
  envelope. Check `success`; on `false`, read the `errors` array and `message`.
  Retry only when `retryableError` is `true`.
- **Warnings:** even on `success: true`, inspect `warnings` for data-quality or
  partial-match notes.
- **Tracing:** log `trackingId` and `auditId` from the response for support and
  audit correlation.
- **Idempotency:** there is no idempotency key; `post_v2_postIdentity` is keyed
  on the source/native id, so re-posting the same record updates it rather than
  duplicating.
