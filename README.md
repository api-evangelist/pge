# Pacific Gas and Electric (pge)

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
