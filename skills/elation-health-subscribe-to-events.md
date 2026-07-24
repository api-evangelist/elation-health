---
name: Subscribe to webhook events
description: Create a webhook subscription to resource updates, verify Ed25519-signed callbacks, and process published events.
api: openapi/elation-event-subscription-api.json
operations: [subscriptions_create, subscriptions_list, subscriptions_retrieve, subscriptions_destroy, published_events_list]
---

# Subscribe to webhook events

Use the Elation Event Subscription API (v2.0) to receive near-real-time notifications.

## Authenticate
Bearer token from `/api/2.0/oauth2/token/` (`client_credentials`); `Authorization: Bearer <token>`.

## Steps
1. **Subscribe** — `subscriptions_create` (`POST /api/2.0/subscriptions/`) with the `resource` (one of 43+ models, e.g. `patients`, `appointments`, `problems`) and your callback URL. The response includes `signing_pub_key`.
2. **List / inspect** — `subscriptions_list` (`GET /api/2.0/subscriptions/`) and `subscriptions_retrieve` (`GET /api/2.0/subscriptions/{id}/`).
3. **Receive callbacks** — Elation POSTs JSON `{data, action, event_id, event_uuid, application_id, resource, resource_url}`. `action` is `"saved"` or `"deleted"`. Your own writes do not trigger callbacks to your app.
4. **Verify the signature** — validate the base64 `El8-Ed25519-Signature` request header against the body using the subscription's `signing_pub_key` (Ed25519). Reject on mismatch.
5. **Backfill/replay** — `published_events_list` (`GET /api/2.0/published_events/`) to enumerate published events.
6. **Unsubscribe** — `subscriptions_destroy` (`DELETE /api/2.0/subscriptions/{id}/`).

## Rules
- Large payloads may arrive as a `resource_url` to fetch rather than inline `data`.
- Prefer `event_uuid` over the integer `event_id` (the id is being migrated).
- See asyncapi/elation-health-events-webhooks.yml.
