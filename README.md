# Pacific Gas and Electric

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Pacific Gas and Electric Company is the investor-owned electric and natural gas utility for northern and central California — incorporated in California in 1905, headquartered in Oakland, roughly 23,000 employees, a 70,000-square- mile service area, about 5.5 million electric accounts and 4.5 million gas accounts, and a subsidiary of PG&E Corporation. It sits in the wires, pipes and metering layer of the United States value chain: a vertically integrated regulated distribution utility that owns the meter and the customer relationship, does not run the wholesale market (CAISO does), and is regulated by the California Public Utilities Commission. Its API posture is the outlier of the American utility sector and deserves to be recorded precisely, because the United States has no federal energy consumer-data mandate at all. PG&E runs Share My Data, its production Green Button Connect My Data implementation, on a live MuleSoft API gateway at https://api.pge.com — a NAESB REQ.21 ESPI 1.1 surface with roughly two dozen documented resources under /GreenButtonConnect/espi/1_1/resource/, an OAuth 2.0 authorization server at /datacustodian/oauth/v2/, and a separate published test environment at /datacustodian/test/oauth/v2/. Unlike almost every other US utility, PG&E publishes the whole contract anonymously: a complete supported-API reference, an OAuth/ESPI authorization guide, a relational data model, supported data elements, function-block scope-string mappings, the ESPI and Share My Data XSD schemas, sample MeterReadings XML, Python and JavaScript SDKs with development guides, published rate limits (one request per second per vendor, 2,000 calls per hour and 20,000 calls per 24 hours per client ID) and a SoapUI walkthrough — all reachable without a login at pge.com. What is not open is the data itself. Share My Data is application-approval gated: a third party needs a US EIN, eligible standing with the CPUC, a CA-issued TLS 1.2 X.509 certificate (self-signed rejected), acceptance of the CPUC-filed Customer Data Access Tariff, and successful connectivity and OAuth testing before approval, after which every production call runs over mutual TLS with bearer tokens scoped to an individual customer's authorization. The mandate story is equally specific and must not be flattened into "voluntary Green Button": California compels third-party access through state law and CPUC tariff — Public Utilities Code section 8380 (SB 1476, 2010), the Customer Information Service Request for Share My Data tariff form (Cal. P.U.C. Sheet 55826-E, Sample Form 79-1186, Advice 6900-E, effective 1 April 2023), and Electric Rule 24 / Gas Rule 25 for demand response providers — but no federal obligation and no Ontario-style Green Button regulation applies. The consumer-versus-market split is stark: consumer data is a real, verified, standards-conformant API behind consent and approval, while PG&E publishes no open market or grid API whatsoever. Its aggregated ZIP-code electric and gas usage datasets, released quarterly under CPUC Decision 14-05-016 through the Energy Data Request Program, are CSV-in- ZIP downloads behind an organization/name/email form and a data-use agreement, not an anonymous feed; California wholesale market data belongs to CAISO.

**Areas:** Energy · United States · Utilities · Electricity · Gas · California · Smart Metering · Green Button · ESPI · Energy Data · Grid · Demand Response

## Pacific Gas and Electric on APIs.io

Everything profiled for Pacific Gas and Electric is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/pge/](https://apis.io/providers/pge/)** — or go straight to one of the APIs or properties below.

### APIs (7)

- [PG&E Share My Data OAuth 2.0 Authorization API](https://apis.io/apis/pge/pge-share-my-data-oauth-api/) — The OAuth 2.0 authorization server behind Share My Data, implemented to meet the NAESB ESPI authorization profile. Two token classes are issued separately:…
- [Pacific Gas and Electric Application Information API](https://apis.io/apis/pge/pge-applicationinformation-api/) — Application Information endpoints
- [Pacific Gas and Electric Authorization API](https://apis.io/apis/pge/pge-authorization-api/) — Authorization endpoints
- [Pacific Gas and Electric Batch API](https://apis.io/apis/pge/pge-batch-api/) — Batch data transfer endpoints
- [Pacific Gas and Electric Usage Point API](https://apis.io/apis/pge/pge-usagepoint-api/) — Usage Point endpoints
- [Pacific Gas and Electric Subscriptions API](https://apis.io/apis/pge/pge-subscriptions-api/) — Manage data subscriptions for customer accounts.
- [Pacific Gas and Electric Usage API](https://apis.io/apis/pge/pge-usage-api/) — Retrieve energy usage interval data.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Pge Share My Data Notifications](https://apis.io/asyncapis/pge/pge-share-my-data-notifications/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Pge Agentic Access](https://apis.io/agentic-access/pge/pge-agentic-access/)

#### Collections (1)

Postman and Open Collection request collections.

- [Green Button API Documentation](https://apis.io/collections/pge/open-green-button-alliance-espi/)

#### Overlays (3)

OpenAPI overlays, including conversational phrasing for each operation.

- [Pg And E Subscriptions Api Overlay](https://apis.io/overlays/pge/pg-and-e-subscriptions-api-overlay/)
- [Pg And E Usage Api Overlay](https://apis.io/overlays/pge/pg-and-e-usage-api-overlay/)
- [Pge Green Button Alliance Espi Overlay](https://apis.io/overlays/pge/pge-green-button-alliance-espi-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Pg And E Authentication](https://apis.io/security/pge/pg-and-e-authentication/)
- [Pge Authentication](https://apis.io/security/pge/pge-authentication/)
- [Pge Domain Security](https://apis.io/security/pge/pge-domain-security/)
- [Pge Vulnerability Disclosure](https://apis.io/security/pge/pge-vulnerability-disclosure/)

#### OAuth scopes (2)

The permissions the API asks for.

- [Pg And E Scopes](https://apis.io/scopes/pge/pg-and-e-scopes/)
- [Pge Scopes](https://apis.io/scopes/pge/pge-scopes/)

#### Rate limits (1)

Published request limits.

- [Pge Rate Limits](https://apis.io/rate-limits/pge/pge-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.pge.com/)
- [Documentation](https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html)
- [Getting started](https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html#getstarted)
- [API reference](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/Supported-APIs.pdf)
- [Sign up](https://sharemydata.pge.com/)
- [Blog](https://www.pge.com/en/newsroom/currents.html)
- [Security](https://www.pge.com/en/about/company-information/vulnerability-disclosure-policy.html)
- [Terms of service](https://www.pge.com/assets/pge/docs/save-energy-and-money/energy-savings-programs/ShareMyData_Platform_TermsofUse.pdf)
- [Privacy policy](https://www.pge.com/en/privacy-center.html)
- [GitHub](https://github.com/pgetech)

## API Evangelist rating

**Kin Score 55.5** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 33.7 |
| Contract governance | 18.2 |
| Operational transparency | 68.4 |
| Developer ergonomics | 82.7 |
| Access clarity | 34.2 |

**Agent readiness 38.6** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | na |
| MCP Server | — |
| Auth Clarity | negotiable (partial credit) |
| Idempotency | na |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | na |
| Delegated Identity | documented (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 50.0** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/pge/](https://apis.io/providers/pge/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Pacific Gas and Electric: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/pge/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
