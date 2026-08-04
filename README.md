# Pacific Gas and Electric (pge)

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

Pacific Gas and Electric Company is the investor-owned electric and natural gas utility for northern and central California — incorporated in California in 1905, headquartered in Oakland, roughly 23,000 employees, a 70,000-square-mile service area, about 5.5 million electric accounts and 4.5 million gas accounts, and a subsidiary of PG&E Corporation. It sits in the wires, pipes and metering layer of the United States value chain: a vertically integrated regulated distribution utility that owns the meter and the customer relationship, does not run the wholesale market (CAISO does), and is regulated by the California Public Utilities Commission.

Its API posture is the outlier of the American utility sector. The United States has no federal energy consumer-data mandate, yet PG&E runs **Share My Data**, a production Green Button Connect My Data implementation, on a live MuleSoft API gateway at `https://api.pge.com` — a NAESB REQ.21 ESPI 1.1 surface with roughly two dozen documented resources, an OAuth 2.0 authorization server, and a separate published test environment. Unlike almost every other US utility, PG&E publishes the whole contract anonymously without a login: a complete supported-API reference, an OAuth/ESPI authorization guide, a relational data model, function-block scope-string mappings, the ESPI and Share My Data XSD schemas, a worked sample payload, Python and JavaScript SDKs with development guides, published rate limits and a SoapUI walkthrough.

What is not open is the data. Share My Data is application-approval gated, and PG&E publishes no open market or grid API at all.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/pge/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/pge/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Utilities
- Electricity
- Gas
- California
- Smart Metering
- Green Button
- ESPI
- Energy Data
- Grid
- Demand Response
- Investor-Owned Utility

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## Mandate and access at a glance

| | |
|---|---|
| **Mandate regime** | `other` — California state, not federal, not Ontario. Public Utilities Code §8380 (SB 1476, 2010); CPUC-filed Customer Data Access Tariff (Electric Sample Form 79-1186, CISR-SMD, Cal. P.U.C. Sheet 55826-E, Advice 6900-E, effective 2023-04-01); Electric Rule 24 / Gas Rule 25; CPUC D.14-05-016 for aggregated data. |
| **Mandate status** | `live-implemented` — verified by differential HTTP probing on 2026-07-27, not by any claim. |
| **Data standard** | Green Button Connect My Data on NAESB REQ.21 ESPI 1.1, Green Button Atom/XML. |
| **Consumer data API** | Yes — real, documented, standards-conformant, consent- and approval-gated. |
| **Open market data** | No — no open grid or system API; aggregated ZIP-code datasets are form- and licence-gated file downloads. |
| **Access gate** | `application-approval` — US EIN, CPUC standing, public-CA X.509 certificate, tariff acceptance, 90-day registration testing, then approval. |
| **Auth model** | OAuth 2.0 (`authorization_code`, `client_credentials`, `refresh_token`) over mandatory mutual TLS 1.2 with client X.509 certificates. No OpenID Connect — `/.well-known/openid-configuration` is 404 everywhere. |
| **Home market** | United States |

## APIs

### PG&E Share My Data (Green Button Connect My Data) ESPI API

PG&E's production Green Button Connect My Data implementation. A NAESB REQ.21 ESPI 1.1 REST API returning Green Button Atom/XML, serving customer-authorized electric and gas interval usage, usage summaries, reading types, billing and account information, service tariff and rate-plan detail, and demand response program enrollment. Verified live anonymously on 2026-07-27: ESPI resource paths answer HTTP 400 `"Invalid Certificate"` from a `Mulesoft-API-Gateway` while deliberately invented sibling paths answer HTTP 404 `No listener for endpoint` — which distinguishes a real mTLS-protected listener from a catch-all.

- **Human URL:** [https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html](https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html)
- **Base URL:** `https://api.pge.com/GreenButtonConnect/espi/1_1/resource`

#### Tags

- Green Button
- ESPI
- Energy Data
- Smart Metering
- Electricity
- Gas
- Demand Response

#### Properties

- [Documentation — Share My Data overview](https://www.pge.com/en/save-energy-and-money/energy-usage-and-tips/understand-my-usage/share-my-data.html)
- [API Reference — Supported APIs (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-APIs.pdf)
- [API Reference — Supported APIs, Click-Thru 2.0 (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-APIs-Click-Thru-2.0.pdf)
- [Documentation — Supported Data Elements (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-Data-Elements.pdf)
- [Documentation — Supported Relational Data Model (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-Relational-Data-Model.pdf)
- [Documentation — Process Flow (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Process-Flow.pdf)
- [XSD](schemas/pge-share-my-data-atom.xsd) — ESPI Atom schema
- [XSD](schemas/pge-share-my-data-espiDerived-20180821.xsd) — espiDerived 2018-08-21
- [XSD](schemas/pge-share-my-data-retailcustomer-20180802.xsd) — retailcustomer 2018-08-02
- [XSD](schemas/pge-share-my-data-click-thru-2.0-espiDerived-20180315.xsd) — Click-Thru 2.0 espiDerived
- [XSD](schemas/pge-share-my-data-click-thru-2.0-retailcustomer-20180315.xsd) — Click-Thru 2.0 retailcustomer
- [OpenAPI](openapi/green-button-alliance-espi-openapi.json) — the Green Button Alliance's ESPI contract, **not** PG&E's; see `review.yml`
- [Example — MeterReadings sample XML](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/MeterReadings_Example.zip)
- [SDK — Python](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/pythonsdk.zip)
- [SDK — JavaScript](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/javascriptsdk.zip)
- [Documentation — Python development guide (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/smd-python-development-guide.pdf)
- [Documentation — JavaScript development guide (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/smd-javascript-development-guide.pdf)
- [Documentation — SoapUI steps (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/soap-ui-steps.pdf)
- [Terms of Service — Share My Data Platform Terms of Use (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/ShareMyData_Platform_TermsofUse.pdf)
- [Registration](https://sharemydata.pge.com/)
- [Reference — CPUC-filed CISR-SMD tariff form (PDF)](https://www.pge.com/tariffs/assets/pdf/tariffbook/ELEC_FORMS_79-1186.pdf)

### PG&E Share My Data OAuth 2.0 Authorization API

The OAuth 2.0 authorization server behind Share My Data, implemented to meet the NAESB ESPI authorization profile. Two token classes are issued separately: a client access token via `client_credentials` for third-party-level resources, and a per-customer access/refresh token pair via `authorization_code`. Authorized scope is returned as an ESPI function-block string. PG&E publishes both a production authorization server and a separate test authorization server used during mandatory registration testing. Verified live anonymously on 2026-07-27: `GET` on the token endpoint returns HTTP 405 `{"error":"invalid_request","error_description":"GET not permitted"}`.

- **Human URL:** [https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html](https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html)
- **Base URL:** `https://api.pge.com/datacustodian/oauth/v2`

#### Tags

- OAuth
- Authorization
- Green Button
- ESPI
- Security

#### Properties

- [Documentation — OAuth Authorization ESPI (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/OAuth_Authorization_ESPI.pdf)
- [Documentation — Supported Function Block Scope String Mapping (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-Function-Block-Scope-String-Mapping-Click-Thru-2.0.pdf)
- [API Reference — Supported APIs (PDF)](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-APIs.pdf)
- [Documentation — Third-party companies / developer resources](https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html)
- [Registration](https://sharemydata.pge.com/)
- [Reference — NAESB ESPI](http://naesb.org/espi)

## Common Properties

- [Website](https://www.pge.com/)
- [Documentation](https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html)
- [Registration](https://sharemydata.pge.com/)
- [GitHub Organization](https://github.com/pgetech)
- [LinkedIn](https://www.linkedin.com/company/pacificgasandelectric)
- [Blog](https://www.pge.com/en/newsroom/currents.html)
- [Vulnerability Disclosure](https://www.pge.com/en/about/company-information/vulnerability-disclosure-policy.html)
- [Privacy](https://www.pge.com/en/privacy-center.html)
- [About](https://www.pge.com/en/about/company-information/company-profile.html)
- [Reference — Energy Data Request Program](https://pge-energydatarequest.com/)
- [Reference — Green Button](https://www.greenbuttondata.org/)

## Notes

- `https://www.pge.com/developers` is a **trap**: it redirects to a Building and Renovation page for property developers, not software developers. The real developer surface is the third-party-companies page linked above.
- PG&E publishes **no first-party OpenAPI, Swagger, Postman collection, GraphQL schema or AsyncAPI**. Its machine-readable contract is XSD; its API reference is PDF. The single file in `openapi/` is the Green Button Alliance's description of the standard, harvested verbatim and labelled as such in its own `info` block.
- Rate limits are published: one request per second per vendor across all client IDs; 2,000 calls per hour per client ID; 20,000 calls per 24 hours per client ID, resetting at 5 p.m. Pacific Time.
- Full probe log, control probes, mandate analysis and the consumer-versus-market split are in [`review.yml`](review.yml).

## Maintainers

- Kin Lane — kin@apievangelist.com
