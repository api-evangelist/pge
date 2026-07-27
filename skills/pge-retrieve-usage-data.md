---
name: PG&E Share My Data — retrieve a customer's interval usage and billing
description: >-
  Walk an authorized customer's ESPI tree on PG&E Share My Data — Subscription
  to UsagePoint to MeterReading to IntervalBlock, plus UsageSummary and
  ReadingType — and filter it correctly with Zulu date windows.
api: openapi/green-button-alliance-espi-openapi.json
provider: Pacific Gas and Electric
generated: '2026-07-27'
method: generated
source: >-
  Grounded in operationIds verified in
  openapi/green-button-alliance-espi-openapi.json and in the PG&E resource paths
  published in Supported-APIs.pdf (reproduced in review.yml documentedEndpoints).
operations:
  - findUsagePoints
  - getUsagePoint
---

# Retrieve usage data from PG&E Share My Data

## Preconditions

A live customer `access_token` from the authorization-code flow (see the
onboard-and-authorize skill), your client certificate for mutual TLS, and a
granted scope that actually includes the data you want. Check the function-block
string first: `FB=4` interval usage, `FB=5` electric interval, `FB=10` gas,
`FB=15` usage summary, `FB=16` usage summary with cost, `FB=46`/`FB=47`
customer/account/program data. Asking for data outside the grant returns `403`.

## The resource tree

Every customer-level path is prefixed with the Subscription:

```
/espi/1_1/resource/Subscription/{SubscriptionID}/UsagePoint
/espi/1_1/resource/Subscription/{SubscriptionID}/UsagePoint/{UsagePointID}
    .../MeterReading
    .../MeterReading/{MeterReadingID}
    .../MeterReading/{MeterReadingID}/IntervalBlock
    .../MeterReading/{MeterReadingID}/IntervalBlock/{IntervalBlockID}
    .../UsageSummary
    .../UsageSummary/{UsageSummaryID}
```

In the harvested standard contract these correspond to `findUsagePoints` (the
feed) and `getUsagePoint` (a single point). Base host:
`https://api.pge.com/GreenButtonConnect`.

## Step 1 — list the customer's usage points

`findUsagePoints` under the Subscription. One UsagePoint per authorized service
agreement (electric or gas). Read the customer class indicator (`RES`, `AGR`,
`COM/IND`, `Other`) and, since the 2024 Meter ID Phase 2 release, the individual
Meter ID — a single service agreement can have multiple meters.

## Step 2 — resolve the reading semantics

`GET /espi/1_1/resource/ReadingType/{ReadingTypeID}` (customer `access_token`)
for commodity, unit of measure, accumulation behaviour, currency and the
power-of-ten multipliers. Interval values are meaningless without it.

`GET /espi/1_1/resource/LocalTimeParameters/{LocalTimeParametersID}` for the
time zone and daylight-saving parameters used to interpret interval timestamps.

## Step 3 — walk MeterReading then IntervalBlock

`GET .../UsagePoint/{UsagePointID}/MeterReading` then
`.../MeterReading/{MeterReadingID}/IntervalBlock`. IntervalBlocks carry the
actual interval readings with reading-quality indicators — including value `19`
("revenue quality: valid and acceptable for billing purposes"), which PG&E notes
is an extension beyond the original ESPI standard.

If you cached a `MeterReadingID` from before March 2023, re-fetch the
MeterReading feed: PG&E re-encoded those identifiers in the Meter ID releases.

## Step 4 — filter by date window, in Zulu only

```
?published-min={ISO8601Z}&published-max={ISO8601Z}
?updated-min={ISO8601Z}&updated-max={ISO8601Z}
```

`published-*` filters by publication window; `updated-*` (added in Release 3.0,
2018-11-15) returns everything that CHANGED in the window, which is what you
want for incremental sync and corrections. PG&E states that non-Zulu date
formats are not supported. There is no cursor, page or limit parameter — the
date window is the pagination.

## Step 5 — billing

`GET .../UsagePoint/{UsagePointID}/UsageSummary` for billed total usage and
billed demand; dollar cost appears only when `FB=16` was granted.

## Handling responses

- Responses are ESPI Atom/XML in the `http://naesb.org/espi` namespace. There is
  no JSON representation. Validate against the XSDs in `schemas/`.
- `HTTP 204` means no data available for the requested range — widen or shift the
  window, do not retry the same call.
- `HTTP 400 Invalid Certificate` means the mutual TLS handshake failed, not that
  your request was malformed.
- `HTTP 403` almost always means the wrong token class or an unauthorized
  function block.
- Full catalog: `errors/pge-problem-types.yml`.

## Rate discipline

One request per second per vendor across all your client IDs. Walking a large
customer's interval tree serially is the intended shape; parallelising it will
throttle you. For large ranges use the asynchronous Batch family instead (see
the batch-and-notifications skill).
