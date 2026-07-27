---
name: Track inventory and article moves
description: Read and adjust on-hand inventory and audit article moves in and out of an Instock site.
api: openapi/instock-openapi.json
operations: [getAllInventory, retrieveMultipleArticlesInventory, adjustArticleInventory, getInventoryArticleId, getAllArticlesMoves, getArticleIdMove]
---

# Track inventory and article moves at an Instock site

Use this skill to reconcile stock levels and audit the flow of articles through an Instock ASRS site.

## Prerequisites
- Bearer token (JWT) in the `Authorization` header (issued by Instock during onboarding).
- A known `site_id` (from `GET /sites`).

## Steps
1. **List all inventory** — `GET /{site_id}/inventory` (`getAllInventory`). Paginate with `start_cursor` / `page_size`.
2. **Batch lookup** — `POST /{site_id}/inventory` (`retrieveMultipleArticlesInventory`) to fetch quantities for a specific set of articles.
3. **Single article** — `GET /{site_id}/inventory/{article_id}` (`getInventoryArticleId`) for one SKU's on-hand quantity.
4. **Adjust** — `PUT /{site_id}/inventory` (`adjustArticleInventory`) to correct inventory quantities.
5. **Audit moves** — `GET /{site_id}/moves` (`getAllArticlesMoves`) for all article flow in/out of the site; `GET /{site_id}/moves/{article_id}` (`getArticleIdMove`) for one article's movement history.

## Rules
- All quantities and identifiers are scoped to the `site_id`.
- Filter move history with the documented timestamp parameters (RFC 3339).
- Handle the `{code, message}` error envelope; log the `Instock-Request-ID` response header for support.
