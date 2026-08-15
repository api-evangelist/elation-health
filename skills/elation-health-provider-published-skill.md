---
name: Elationhealth
description: Use when building integrations with Elation's REST or FHIR APIs, managing clinical workflows (patient charts, visit notes, prescriptions, labs), configuring practice operations (billing, scheduling, insurance), or troubleshooting EHR functionality. Agents should reach for this skill when working with healthcare data exchange, appointment management, patient documentation, medication ordering, or compliance reporting.
metadata:
    mintlify-proj: elationhealth
    version: "1.0"
---

# Elation Health EHR Skill

## Product Summary

Elation Health EHR is a cloud-based electronic health record system for medical practices. It provides clinical documentation (visit notes, patient charts), prescription management (including ePrescribing and controlled substances via EPCS), appointment scheduling, billing and claims processing, lab/imaging ordering, and patient communication tools. Agents use Elation to manage patient data, automate clinical workflows, integrate with third-party systems via REST/FHIR APIs, and ensure compliance with healthcare regulations (MIPS, Cures Act, HIPAA). Key entry points: REST API at `/articles/rest/overview/`, FHIR API at `/articles/fhir/`, EHR UI documentation at `/elation-ehr`, Billing system at `/elation-billing`. CLI commands are not applicable; Elation is web-based and API-driven.

## When to Use

Reach for this skill when:

- **Building API integrations**: Connecting third-party systems (PMS, RCM, patient engagement tools) to Elation via REST or FHIR APIs
- **Managing patient workflows**: Creating/updating patient charts, demographics, insurance, visit notes, medications, lab orders
- **Configuring practice settings**: Setting up providers, service locations, billing codes, appointment types, user accounts
- **Troubleshooting clinical operations**: Resolving ePrescribing failures, billing errors, insurance eligibility issues, fax failures
- **Implementing compliance**: MIPS reporting, Cures Act data exchange, HIPAA audit logs, quality measure tracking
- **Automating workflows**: Using webhooks to sync appointments/billing, automating visit note templates, coding automation
- **Data exchange**: Exporting CCDA, FHIR bulk data, or sharing data via Carequality, Direct messaging, or patient portals

Do not use this skill for: Account creation, pricing inquiries, sales support, or non-technical administrative tasks.

## Quick Reference

### API Authentication
- **OAuth 2.0**: All API calls require Bearer token via `POST /oauth2/token/`
- **Grant type**: `client_credentials` (system-to-system) or `authorization_code` (user-facing apps)
- **Scopes**: Default `apiv2` grants all resources; use `act_as_user` for user impersonation
- **Token expiry**: Check `expires_in` field; refresh tokens before expiry

### REST API Categories
| Category | Purpose | Key Endpoints |
|----------|---------|---------------|
| Patient Profile API | Demographics, allergies, problems, vitals, immunizations | `/patients/`, `/allergies/`, `/problems/` |
| Patient Document API | Visit notes, medications, labs, reports, referrals | `/visit-notes/`, `/medications/`, `/reports/` |
| Orders API | Lab, imaging, cardiac, pulmonary, sleep orders | `/lab-orders/`, `/imaging-orders/` |
| Scheduling API | Appointments, appointment types, recurring events | `/appointments/`, `/appointment-types/` |
| Billing API | Superbills, billing codes, bills | `/bills/`, `/billing-codes/` |
| Insurance API | Insurance companies, plans, patient policies | `/insurance-companies/`, `/insurance-plans/` |
| Practice API | Providers, locations, staff, contacts | `/practices/`, `/physicians/`, `/service-locations/` |
| Messaging API | Secure in-app messaging threads | `/message-threads/`, `/thread-messages/` |

### FHIR API Base URLs
- **Sandbox**: `https://sandbox-fhir.elationemr.com/fhir/r4/`
- **Production**: `https://fhir.elationemr.com/fhir/r4/`
- **Standard**: FHIR R4 v4.0.1, US Core v5.0.1, SMART on FHIR 1.0.0

### Common File Paths & Configuration
- **Practice settings**: Settings > Practice Settings (providers, locations, fee schedules, claim defaults)
- **User management**: Settings > User Accounts (roles, permissions, MFA, SSO)
- **Billing configuration**: Settings > Billing Settings (service locations, procedure codes, claim submission)
- **API credentials**: Settings > API Access (self-service credential generation for REST/FHIR)
- **Patient demographics**: Patient chart > Patient name > Demographics (insurance, contact, tags)

### Essential Commands (UI-based, not CLI)
- **Create patient**: New Chart > Enter demographics > Save
- **Create visit note**: Appointment > Document > Select template > Sign
- **ePrescribe**: Rx > Prescription Form > Select medication > Choose pharmacy > Prescribe
- **Order lab**: Orders > Lab Order > Select tests > Sign
- **Run eligibility**: Patient insurance > Run Eligibility (real-time check via ClaimMD)
- **File claim**: Billing > Create Superbill > Add charges > File Claim
- **Send referral**: Letters > Create Referral > Select provider > Sign & Send

## Decision Guidance

### When to Use REST API vs FHIR API

| Scenario | Use REST API | Use FHIR API |
|----------|-------------|------------|
| Custom integration with Elation | ✓ | |
| Interoperability with other EHRs | | ✓ |
| Bulk data export for analytics | | ✓ (Bulk Export) |
| Real-time appointment sync | ✓ (Webhooks) | |
| Patient-facing app (SMART on FHIR) | | ✓ |
| Writing coded clinical data | ✓ (LOINC, CVX, NDC) | ✓ |
| Compliance with 21st Century Cures | | ✓ |

### When to Use Manual vs Automated Workflows

| Task | Manual | Automated |
|------|--------|-----------|
| Prescription refills | Staff refill one-by-one | Bulk refill for multiple meds |
| Visit note creation | Type/dictate notes | Apply template to appointment |
| Coding | Manual CPT/ICD-10 entry | Automatic coding for BMI, BP, PHQ-9 |
| Insurance eligibility | Call payer or check portal | Real-time eligibility check (ClaimMD) |
| Lab results | Manually file documents | Lab interface auto-import |
| Appointment reminders | Manual phone/email | Automated SMS/email reminders |

### When to Use Controlled Substances (EPCS) vs Regular Prescriptions

| Requirement | Regular Rx | Controlled Substance (EPCS) |
|-------------|-----------|---------------------------|
| Requires pharmacy selection | Yes | Yes (EPCS-certified only) |
| Requires provider signature | Yes | Yes + 2FA token/security code |
| Staff can send on behalf | Yes (if Rx Delegate) | No (DEA regulation) |
| Can be printed | Yes | Yes (tamper-proof paper required) |
| Requires setup | Provider Account Authentication | EPCS sign-up + identity proofing + token |

## Workflow

### Typical Task: Create Patient Chart and Order Lab

1. **Understand the request**: Determine if patient is new or existing; identify required demographics (name, DOB, sex, address, insurance).

2. **Check existing content**: Search patient by name/DOB to avoid duplicates. If found, update demographics instead of creating new chart.

3. **Create or update chart**:
   - New: Click New Chart > Enter first/last name, DOB, sex, address, phone, email
   - Existing: Open chart > Click patient name > Edit demographics
   - Add insurance: Scroll to Insurance section > Add primary/secondary/tertiary > Verify coverage with Real-Time Eligibility

4. **Create lab order**:
   - Click Orders > Lab Order > Select lab vendor (Quest, LabCorp, etc.)
   - Search and select tests (e.g., "CBC", "CMP")
   - Add diagnosis codes (ICD-10) at test level if required
   - Review order > Sign > Transmit to lab

5. **Verify completion**:
   - Confirm patient chart saved with complete demographics
   - Confirm lab order shows "Signed" status in Chronological Record
   - Check for lab results in Results section (auto-imported if interface enabled)

### Typical Task: ePrescribe Medication

1. **Verify prerequisites**: Patient has address on file; provider has completed Provider Account Authentication; pharmacy is in Surescripts network.

2. **Open prescription form**: Patient chart > Rx > Prescription Form (Rx/OTC/CS).

3. **Enter medication details**:
   - Search medication by name/strength (e.g., "Lisinopril 10mg")
   - If not found, manually add (note: no NDC, no formulary info)
   - Enter Sig (e.g., "1 tablet daily"), Qty, Refills, Days Supply
   - Add diagnosis codes (up to 2 ICD-10 codes)
   - Check Drug Decision Support for interactions

4. **Select pharmacy**: Search by name, city, ZIP, or phone. Verify EPCS label if controlled substance.

5. **Send prescription**:
   - Click Prescribe > Review Prescription Summary
   - For controlled substances: Enter Token Password + 6-digit Security Code from VIP Access App/key fob
   - Click Sign + Send
   - Monitor status in Chronological Record (Sent, Failed, or Pending)

6. **Troubleshoot if failed**:
   - Verify patient address is complete (not blank, not just "UNKNOWN")
   - Confirm pharmacy is EPCS-certified (for controlled substances)
   - Check provider NPI and license are current
   - Retry or fall back to print/call workflow

### Typical Task: Build REST API Integration

1. **Register as developer**: Submit form at help.elationhealth.com to access sandbox environment.

2. **Generate credentials**: Settings > API Access > Create new credential > Select scopes (e.g., `apiv2` for all resources, or specific scopes like `system/visit_notes.read`).

3. **Obtain access token**:
   ```
   POST /oauth2/token/
   Content-Type: application/x-www-form-urlencoded
   
   grant_type=client_credentials&client_id=YOUR_ID&client_secret=YOUR_SECRET&scope=apiv2
   ```
   Response includes `access_token`, `expires_in`, `refresh_token`.

4. **Make API calls**: Include Bearer token in Authorization header:
   ```
   GET /patients/?first_name=John&last_name=Doe
   Authorization: Bearer YOUR_ACCESS_TOKEN
   ```

5. **Handle pagination**: Use `limit` and `offset` parameters; check `next` field in response for more results.

6. **Subscribe to webhooks** (optional): Use Event Subscription API to listen for patient, appointment, or bill updates instead of polling.

7. **Test in sandbox**: Verify all endpoints and error handling before moving to production.

8. **Request production credentials**: Contact partnerships@elationhealth.com with integration details; Elation support will provision production access.

## Common Gotchas

- **Duplicate patient charts**: Always search before creating. Elation prevents duplicates by matching first_name, last_name, DOB, and sex; if you create a chart with matching demographics, you'll get a 409 Conflict error.

- **Missing patient address for ePrescribing**: Patient address is required for both regular and controlled substance prescriptions. If patient is unhoused, use "HOMELESS" in address field with local city/state/ZIP. If address unknown, use "UNKNOWN" with local city/state/ZIP.

- **Pharmacy not in Surescripts network**: Elation cannot add new pharmacies; ask the pharmacy to enroll with Surescripts directly. Use print/call workflow as fallback.

- **EPCS token expiry**: EPCS tokens expire and must be renewed. Check token status in Settings > EPCS. Expired tokens block controlled substance prescribing.

- **Provider Account Authentication not completed**: Providers must complete identity proofing and account setup before ePrescribing. Check Settings > Account > Provider Account Authentication status.

- **Controlled substances cannot be sent by staff delegates**: DEA regulations require the prescriber to sign with their own 2FA token. Staff can draft and save for provider review, but cannot send.

- **Signed documents cannot be updated**: Once a visit note, prescription, or lab order is signed, it cannot be edited. You must create an amendment or new document.

- **Insurance eligibility checks have limits**: Real-time eligibility (ClaimMD) has per-check costs. Manual verification via payer portal is free but must be documented in Notes field.

- **Webhooks don't fire for self-made changes**: If your app updates a patient record via API, the webhook won't fire back to your app (prevents echo loops). Monitor via polling or separate audit logs.

- **API token scope mismatch**: If your token doesn't include the required scope (e.g., `system/visit_notes.write`), you'll get a 403 Forbidden. Request the correct scope when generating credentials.

- **Billing claims require complete demographics**: Claims fail if patient is missing address, insurance, or provider NPI. Always validate before filing.

- **Lab orders without diagnosis codes**: Some payers require diagnosis codes at the test level. Add ICD-10 codes during order creation to avoid rejection.

- **Visit note templates don't auto-apply**: Templates must be manually selected or set up via Visit Note Automation (requires appointment type configuration).

- **Fax failures are common**: Verify recipient fax number has dial tone, check for duplicate service lines in claims, and ensure all required fields are populated. Retry after confirming recipient equipment.

## Verification Checklist

Before submitting work:

- [ ] **Patient data**: Verified no duplicate charts exist; demographics include address, phone, email, insurance
- [ ] **Prescriptions**: Patient has address on file; pharmacy is in Surescripts network; provider completed Provider Account Authentication
- [ ] **Controlled substances**: EPCS token is current; provider has 2FA device; pharmacy is EPCS-certified
- [ ] **Lab orders**: Diagnosis codes added at test level if required; lab vendor interface is enabled; order is signed
- [ ] **Billing**: Superbill includes provider NPI, service location, diagnosis codes, and procedure codes; patient insurance is verified
- [ ] **API integration**: Access token includes required scopes; error handling covers 401 (expired token), 403 (insufficient scope), 409 (duplicate), 429 (rate limit), 503 (service unavailable)
- [ ] **Webhooks**: Event subscription is active; webhook URL is reachable; payload parsing handles all expected fields
- [ ] **Compliance**: MIPS measures are tracked; audit logs are enabled; patient consent is documented for data sharing
- [ ] **Testing**: Verified in sandbox environment before production; tested error scenarios (missing data, network failures, expired tokens)

## Resources

**Comprehensive navigation**: Visit https://help.elationhealth.com/llms.txt for a complete page-by-page listing of all documentation.

**Critical documentation pages**:
1. [REST API Overview](https://help.elationhealth.com/articles/rest/overview/api-overview) — All API categories, endpoints, and getting started
2. [FHIR API Getting Started](https://help.elationhealth.com/articles/fhir/getting-started-with-standardized-api) — FHIR R4, SMART on FHIR, bulk export
3. [Elation EHR Overview](https://help.elationhealth.com/elation-ehr) — Clinical workflows, billing, scheduling, compliance
4. [OAuth & Authentication](https://help.elationhealth.com/articles/rest/overview/oauth) — Token generation, scopes, user impersonation
5. [Troubleshooting ePrescribing](https://help.elationhealth.com/articles/Troubleshooting-ePrescribing-Errors-for-Regular-Medications-in-Elation-EHR) — Common prescription errors and fixes
6. [Billing & Claims](https://help.elationhealth.com/billing-claims-ehr) — Superbills, coding, insurance eligibility, claim filing

---

> For additional documentation and navigation, see: https://help.elationhealth.com/llms.txt