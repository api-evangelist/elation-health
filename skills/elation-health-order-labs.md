---
name: Create and track a lab order
description: Create a laboratory order for a patient and retrieve its status and printable form.
api: openapi/elation-orders-api.json
operations: [lab_orders_create, lab_orders_retrieve, lab_orders_list, lab_orders_printable_retrieve]
---

# Create and track a lab order

Use the Elation Orders API (v2.0).

## Authenticate
Bearer token from `/api/2.0/oauth2/token/` (`client_credentials`); `Authorization: Bearer <token>`.

## Steps
1. **Create the lab order** — `lab_orders_create` (`POST /api/2.0/lab_orders/`) with `patient`, ordering `physician`, `practice`, and the tests/vendor. Returns the order `id`.
2. **Retrieve** — `lab_orders_retrieve` (`GET /api/2.0/lab_orders/{id}/`).
3. **List** — `lab_orders_list` (`GET /api/2.0/lab_orders/`) filtered by patient; cursor pagination.
4. **Printable requisition** — `lab_orders_printable_retrieve` (`GET /api/2.0/lab_orders/{id}/printable/`) returns the printable form.

## Rules
- Cardiac/imaging/pulmonary/sleep orders have parallel operations (e.g. `imaging_orders_create`) in the same spec.
- Handle 400/403/404/413 (payload too large)/429 per errors/elation-health-problem-types.yml.
