---
name: Register and read a patient
description: Create a new patient record in Elation and read it back, including demographics and clinical profile.
api: openapi/elation-patient-profile-api.json
operations: [patients_create, patients_retrieve, patients_list, patients_partial_update]
---

# Register and read a patient

Use the Elation Patient Profile API (v2.0) to create a patient and retrieve their profile.

## Authenticate
1. POST `client_id` + `client_secret` to `/api/2.0/oauth2/token/` (OAuth2 `client_credentials`) to get a bearer token. Send `Authorization: Bearer <token>` on every call. To act as a specific provider, add the `X-On-Behalf-Of` header (requires the `act_as_user` scope).

## Steps
1. **Create the patient** — `patients_create` (`POST /api/2.0/patients/`). Supply demographics (first_name, last_name, dob, sex) and the `practice` id. Elation returns the new patient `id`.
2. **Read it back** — `patients_retrieve` (`GET /api/2.0/patients/{id}/`).
3. **Find existing patients** — `patients_list` (`GET /api/2.0/patients/`) with filters; iterate with cursor-based pagination (`cursor` param; follow `next`). Max 100 results per page.
4. **Update** — `patients_partial_update` (`PATCH /api/2.0/patients/{id}/`) for a partial change.

## Rules
- Errors are `application/json`; handle 400 (validation), 401 (bad/absent token), 403 (missing scope/practice authorization), 404, 429 (rate limited — back off).
- No idempotency key exists; do not blind-retry `patients_create` on timeout — reconcile with `patients_list` first.
- See conventions/elation-health-conventions.yml and errors/elation-health-problem-types.yml.
