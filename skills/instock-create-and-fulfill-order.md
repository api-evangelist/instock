---
name: Create and fulfill a customer order
description: Create a customer order at an Instock site and drive it through fulfillment status, using the Instock API.
api: openapi/instock-openapi.json
operations: [getSites, addOrder, getOrder, getOrderStatus, advanceOrderStatus, getOrderTask, cancelRegisteredOrder]
---

# Create and fulfill an order at an Instock site

Use this skill to place a customer order against a specific Instock ASRS site and track it to completion.

## Prerequisites
- Bearer token (JWT) in the `Authorization` header (issued by Instock during onboarding).
- The articles referenced by the order must already be uploaded (see the onboard-articles skill).

## Steps
1. **Find the site** — `GET /sites` (`getSites`) to get the `site_id` for the ASRS installation you are ordering against.
2. **Create the order** — `POST /{site_id}/orders` (`addOrder`). Supply your host `order_id` (host-issued, unique within your organization) plus line items and any `attributes`.
3. **Confirm** — `GET /{site_id}/orders/{order_id}` (`getOrder`) to read back the registered order.
4. **Check status** — `GET /{site_id}/orders/{order_id}/status` (`getOrderStatus`).
5. **Advance fulfillment** — `POST /{site_id}/orders/{order_id}/status` (`advanceOrderStatus`) to move the order forward.
6. **Inspect a fulfillment chunk** — `GET /{site_id}/ordertasks/{ordertask_id}` (`getOrderTask`) for a specific `ordertask` (Instock-issued unit of order fulfillment).
7. **Cancel if needed** — `DELETE /{site_id}/orders/{order_id}/status` (`cancelRegisteredOrder`) to cancel a registered order.

## Rules
- `site_id` is Instock-issued (global); `order_id` is host-issued (unique per organization); `ordertask_id` is Instock-issued.
- Optionally send `Instock-Correlation-ID` on each request to correlate faulty behavior across your host system.
- Handle the `{code, message}` error envelope (`unauthorized`, `forbidden`, `resource_not_found`, `resource_cannot_be_created`).
