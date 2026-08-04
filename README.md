# Elation Health (elation-health)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Elation Health is a United States clinical-first electronic health record (EHR/EMR) and healthcare technology company, founded in 2010 and headquartered in San Francisco, California, serving independent primary care practices, value-based care organizations, and digital health partners. Beyond its provider-facing EHR, Elation ships a broad, well-documented public REST API (v2.0) - authenticated with OAuth2 - that lets partners read and write clinical and administrative data. Elation also operates login-gated HL7 FHIR R4 / SMART-on-FHIR interoperability endpoints for ONC/CMS 21st Century Cures Act compliance and an MCP server for agentic access.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/elation-health/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/elation-health/refs/heads/main/apis.yml)

## Tags

- Healthcare
- United States
- EHR / EMR
- FHIR / HL7 / SMART on FHIR
- Interoperability
- Primary Care / Value-Based Care

## Timestamps

- **Created:** 2026-07-24
- **Modified:** 2026-07-24

## APIs

The Elation API v2.0 is a REST API documented on a ReadMe developer portal and backed by machine-readable OpenAPI definitions. Base URL: `https://api.app.elationemr.com/api/2.0` (sandbox: `https://sandbox.elationemr.com/api/2.0`). Authentication is OAuth2 (client-credentials bearer tokens) with `apiv2`, `act_as_user`, and `system/{resource}.read|write` scopes.

- **Elation OAuth API** — OAuth2 token endpoint (`/api/2.0/oauth2/token/`) and token scopes. [OpenAPI](openapi/elation-api-authentication.json) · [Docs](https://docs.elationhealth.com/docs/oauth)
- **Elation Patient Profile API** — patients, demographics, allergies, problems, and clinical profile. [OpenAPI](openapi/elation-patient-profile-api.json)
- **Elation Visit Notes API** — clinical visit notes, documentation, and note signing. [OpenAPI](openapi/elation-visit-notes-api.json)
- **Elation Patient Document API** — patient documents, uploads, tagging, and document workflows. [OpenAPI](openapi/elation-patient-document-api.json)
- **Elation Orders API** — laboratory, imaging, and other clinical orders. [OpenAPI](openapi/elation-orders-api.json)
- **Elation Scheduling API** — appointments, appointment types, and provider schedules. [OpenAPI](openapi/elation-scheduling-api.json)
- **Elation Billing API** — bills, billing codes, and outstanding balances. [OpenAPI](openapi/elation-billing-api.json)
- **Elation Insurance API** — insurance companies and plans. [OpenAPI](openapi/elation-insurance-api.json)
- **Elation Patient Insurance API (Premium) & Eligibility** — patient policies, insurance cards, and eligibility checks. [OpenAPI](openapi/elation-premium-patient-insurance-api.json)
- **Elation Practice API** — practices, providers, and service locations. [OpenAPI](openapi/elation-practice-api.json)
- **Elation User Management API** — application users and access. [OpenAPI](openapi/elation-user-management-api.json)
- **Elation Messaging API** — message threads and thread members. [OpenAPI](openapi/elation-messaging-api.json)
- **Elation Event Subscription API** — webhook event subscriptions and published events. [OpenAPI](openapi/elation-event-subscription-api.json) · [Docs](https://docs.elationhealth.com/docs/webhooks)
- **Elation Reference Data API** — shared reference and lookup data. [OpenAPI](openapi/elation-reference-data-api.json)
- **Elation Care Gaps API** — care gap definitions and quality-program data for value-based care. [OpenAPI](openapi/elation-care-gaps-api-1.json)
- **Elation Import API** — bulk clinical and administrative data imports. [OpenAPI](openapi/elation-elation-import-api.json)

## FHIR / Interoperability

Elation operates HL7 FHIR R4 and SMART-on-FHIR endpoints (e.g. `/fhir/R4/metadata`, `/.well-known/smart-configuration`) for ONC/CMS 21st Century Cures Act information-blocking compliance. These are login-gated to registered applications and were not anonymously retrievable, so no FHIR CapabilityStatement was captured.

## Common Properties

- [Website](https://www.elationhealth.com/)
- [Developer Portal](https://docs.elationhealth.com/)
- [API Overview](https://docs.elationhealth.com/docs/api-overview)
- [API Reference](https://docs.elationhealth.com/reference)
- [Getting Started](https://docs.elationhealth.com/docs/getting-started-2)
- [Authentication (OAuth)](https://docs.elationhealth.com/docs/oauth)
- [Webhooks](https://docs.elationhealth.com/docs/webhooks)
- [MCP Server](https://docs.elationhealth.com/docs/mcp)
- [GitHub Organization](https://github.com/elationemr)
- [LinkedIn](https://www.linkedin.com/company/elationhealth/)
- [Blog](https://www.elationhealth.com/blog/)
- [Pricing](https://www.elationhealth.com/pricing/)
- [Sandbox Sign-Up](https://www.elationhealth.com/contact-us/sandbox/)
- [Status Page](https://elationhealth.statuspage.io/)
- [Terms of Use](https://www.elationhealth.com/terms-of-use/)
- [Privacy Policy](https://www.elationhealth.com/privacy-policy/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
