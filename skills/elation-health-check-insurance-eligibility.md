---
name: Check patient insurance eligibility
description: Run an insurance eligibility check for a patient policy and retrieve the full eligibility report.
api: openapi/elation-premium-patient-insurance-api.json
operations: [patient_insurances_eligibility_create, patient_insurances_eligibility_retrieve, patient_insurances_eligibility_full_report_retrieve]
---

# Check patient insurance eligibility

Use the Elation Premium Patient Insurance & Eligibility API (v2.0). This is a premium add-on surface.

## Authenticate
Bearer token from `/api/2.0/oauth2/token/` (`client_credentials`); `Authorization: Bearer <token>`.

## Steps
1. **Run an eligibility check** — `patient_insurances_eligibility_create` (`POST /api/2.0/patient_insurances/{id}/eligibility/`) where `{id}` is the patient insurance policy id.
2. **Retrieve the result** — `patient_insurances_eligibility_retrieve` (`GET /api/2.0/patient_insurances/{id}/eligibility/`).
3. **Full report** — `patient_insurances_eligibility_full_report_retrieve` (`GET /api/2.0/patient_insurances/{id}/eligibility_full_report/`) for the detailed payer response.

## Rules
- Requires an active patient insurance policy (create via the patient insurance endpoints first).
- Handle 400/403 (feature not enabled for the practice / scope)/404/429 per errors/elation-health-problem-types.yml.
