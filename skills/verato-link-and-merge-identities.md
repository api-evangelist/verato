---
name: Link and merge duplicate identities
description: Manually link, merge, unlink, and unmerge person identities to curate golden records in Verato.
api: openapi/verato-person-openapi.yml
operations:
  - post_linkIdentities
  - post_mergeIdentities
  - post_unlinkIdentities
  - post_unmergeIdentities
---

# Link and merge duplicate identities

Curate golden records by asserting or reversing links and merges between person
identities in the Verato Universal Identity Platform.

## Preconditions
- HTTP **Basic** auth over **mutual TLS**; all calls `POST` JSON.

## Steps
1. **Link** two source identities you know represent the same person with
   `post_linkIdentities`.
2. **Merge** identity clusters with `post_mergeIdentities` to collapse duplicate
   resolved identities into one.
3. **Reverse a link** with `post_unlinkIdentities` when a link was wrong.
4. **Reverse a merge** with `post_unmergeIdentities` to split a cluster back
   apart.

## Rules
- These are consequential, state-changing operations on the golden record —
  confirm the identity ids before calling and audit every action.
- **Error handling:** check the `ServiceResponse` `success` flag; on failure read
  `errors` / `message`, and retry only when `retryableError` is `true`.
- **Tracing:** capture `trackingId` and `auditId` for every mutation.
- No idempotency key is available; do not blindly re-issue merges — re-query the
  identity state first.
