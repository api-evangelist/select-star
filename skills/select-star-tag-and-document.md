---
name: select-star-tag-and-document
description: Enrich a Select Star data asset by updating its metadata/description and applying governance tags.
api: Select Star Metadata API
generated: '2026-07-21'
method: generated
source: openapi/select-star-openapi.yml
operations:
- search_list
- metadata_retrieve
- metadata_partial_update
- tags_list
- tags_items_partial_update
---

# Tag and document an asset in Select Star

Improve catalog quality by writing a description and applying tags to a table or
column.

## Prerequisites
- Base URL: `https://api.production.selectstar.com/v1/`
- Header: `Authorization: Token <your_api_token>` (keyword `Token`, not `Bearer`).
- Requires a role with write access (`data_manager` or `admin`).
- Trailing slash required on every path.

## Steps
1. **Locate the asset.** Use `search_list` (`GET /v1/search/`) or a resource list
   to get the asset `guid`.
2. **Read current metadata.** Call `metadata_retrieve`
   (`GET /v1/metadata/{guid}/`) to see the existing description and fields.
3. **Update the description.** Call `metadata_partial_update`
   (`PATCH /v1/metadata/{guid}/`) with the new description (rich text is
   supported per the docs).
4. **Find or create the tag.** Call `tags_list` (`GET /v1/tags/`) to find the
   tag guid (create with `tags_create` if needed).
5. **Apply the tag.** Call `tags_items_partial_update`
   (`PATCH /v1/tags/{guid}/items/`) to attach the tag to the asset.

## Conventions & errors
- Mutations are not idempotent — no idempotency key is supported, so avoid blind
  retries on `PATCH`/`POST`.
- `403` means the token's role lacks write permission.
- Respect the `429` throttle (burst 1000/60s, sustained 10,000/24h).
