---
name: select-star-trace-lineage
description: Find a table or column in the Select Star catalog and trace its column-level data lineage (upstream sources and downstream consumers).
api: Select Star Metadata API
generated: '2026-07-21'
method: generated
source: openapi/select-star-openapi.yml
operations:
- search_list
- tables_list
- tables_retrieve
- lineage_retrieve
---

# Trace data lineage in Select Star

Locate a data asset and walk its lineage graph so you can see where its data
comes from and what depends on it.

## Prerequisites
- Base URL: `https://api.production.selectstar.com/v1/`
- Header: `Authorization: Token <your_api_token>` (use the keyword `Token`, NOT `Bearer`).
- Every path must end with a trailing slash.

## Steps
1. **Find the asset.** Call `search_list` (`GET /v1/search/`) with a query, or
   `tables_list` (`GET /v1/tables/`) filtered with `search_name` /
   `search_description`. Page with `page` and `page_size`. Capture the target
   asset's `guid`.
2. **Confirm the asset.** Call `tables_retrieve` (`GET /v1/tables/{guid}/`) to
   verify the table and read its schema/database context and owners.
3. **Traverse lineage.** Call `lineage_retrieve` (`GET /v1/lineage/{guid}/`) with
   the asset guid to pull upstream and downstream column-level lineage edges.
4. **Recurse as needed.** For any connected asset guid returned, repeat step 3 to
   walk further up or down the lineage graph.

## Conventions & errors
- Handle `429` by backing off the number of seconds in the `detail` message
  (burst 1000/60s, sustained 10,000/24h).
- `401` means a missing/invalid token; `404` often means a missing trailing slash
  or an unknown guid.
