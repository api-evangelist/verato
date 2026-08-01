---
name: Query households and manage relationships
description: Retrieve household groupings and add, search, and delete typed relationships between Verato identities.
api: openapi/verato-person-openapi.yml
operations:
  - post_householdQuery
  - post_addRelationshipService
  - post_searchRelationshipsService
  - post_deleteRelationshipService
---

# Query households and manage relationships

Use the Verato Person API to group related people into households and to manage
typed relationships between resolved identities.

## Preconditions
- HTTP **Basic** auth over **mutual TLS**; all calls `POST` JSON.

## Steps
1. **Find a household** — call `post_householdQuery` with a person identity to
   retrieve the household grouping of related identities.
2. **Add a relationship** — call `post_addRelationshipService` to assert a typed
   relationship between two identities.
3. **Search relationships** — call `post_searchRelationshipsService` to list an
   identity's relationships.
4. **Delete a relationship** — call `post_deleteRelationshipService` to remove
   one.

## Rules
- **Error handling:** inspect the `ServiceResponse` envelope — `success`,
  `errors`, `warnings`, and `retryableError`.
- Paginate household/relationship searches with `pageNumber` / `pageSize` and
  read `totalElements`.
- **Tracing:** record `trackingId` and `auditId`.
