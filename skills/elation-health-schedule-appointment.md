---
name: Schedule a patient appointment
description: Look up appointment types and book, retrieve, and update an appointment for a patient.
api: openapi/elation-scheduling-api.json
operations: [appointment_types_list, appointments_create, appointments_retrieve, appointments_list, appointments_partial_update]
---

# Schedule a patient appointment

Use the Elation Scheduling API (v2.0).

## Authenticate
Obtain a bearer token from `/api/2.0/oauth2/token/` (`client_credentials`) and send `Authorization: Bearer <token>`.

## Steps
1. **List appointment types** — `appointment_types_list` (`GET /api/2.0/appointment_types/`) to pick a valid `appointment_type`.
2. **Book** — `appointments_create` (`POST /api/2.0/appointments/`) with `patient`, `physician`, `practice`, `scheduled_date`, and duration. Returns the appointment `id`.
3. **Retrieve** — `appointments_retrieve` (`GET /api/2.0/appointments/{id}/`).
4. **Search** — `appointments_list` (`GET /api/2.0/appointments/`) filtered by patient/physician/date; page with the `cursor` param.
5. **Reschedule / cancel** — `appointments_partial_update` (`PATCH /api/2.0/appointments/{id}/`) or `appointments_destroy` (`DELETE /api/2.0/appointments/{id}/`).

## Rules
- Handle 400/403/404/429 as in conventions/elation-health-conventions.yml.
- No idempotency key — reconcile via `appointments_list` before retrying a create.
