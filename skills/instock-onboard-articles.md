---
name: Onboard articles (SKUs) into Instock
description: Upload a catalog of articles into Incloud and verify they landed, using the Instock API.
api: openapi/instock-openapi.json
operations: [uploadArticles, listArticles, getArticle, updateArticle]
---

# Onboard articles into Instock (Incloud)

Use this skill to load a product catalog (articles / SKUs) into Instock before running any orders.

## Prerequisites
- A Bearer token (JWT) issued by Instock during onboarding. Send it as `Authorization: <token>`.
- HTTPS only; `Content-Type: application/json`. Property names are `snake_case`; timestamps are RFC 3339.

## Steps
1. **Upload articles** — `POST /articles` (`uploadArticles`). This is the only bulk write surface; you may upload one or many articles in a single request. Include any custom `attributes` your organization agreed with Instock.
2. **List to confirm** — `GET /articles` (`listArticles`). Paginate with `start_cursor` / `page_size` (10–1000); follow `next_cursor` while `has_more` is true.
3. **Retrieve one** — `GET /articles/{article_id}` (`getArticle`) to verify a specific SKU and its attributes.
4. **Update if needed** — `PUT /articles/{article_id}` (`updateArticle`) to correct article data.

## Rules
- `article_id` is host-issued and must be unique within your organization.
- On errors, read the `{code, message}` envelope (e.g. `bad_request`, `resource_cannot_be_created`); capture the `Instock-Request-ID` response header when contacting support.
- There is no idempotency-key mechanism — de-duplicate uploads on your side.
