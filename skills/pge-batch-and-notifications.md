---
name: PG&E Share My Data — asynchronous batch retrieval and notifications
description: >-
  Use the ESPI Batch family and PG&E's Notification URI callback to pull bulk or
  large-range data — request, acknowledge, receive the notification, then GET the
  prepared resource.
api: openapi/green-button-alliance-espi-openapi.json
provider: Pacific Gas and Electric
generated: '2026-07-27'
method: generated
source: >-
  Grounded in the downloadBulkData operationId verified in
  openapi/green-button-alliance-espi-openapi.json and in PG&E's published Batch
  resource paths and notification model
  (https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html,
  Supported-APIs.pdf).
operations:
  - downloadBulkData
---

# Batch retrieval and notifications on PG&E Share My Data

## When to use this instead of walking the tree

Synchronous resource walking is for one customer and a short window. For bulk
across your whole authorized book, or for a long history, use the asynchronous
Batch family. PG&E acknowledges the request, prepares the data, and POSTs a
notification to the Notification URI you registered — the notification carries
retrieval URIs, not the data.

This is ESPI function block `FB=35` (REST for Bulk) plus `FB=39` (Push model:
post notification of data being ready, followed by GET).

## The Batch resources

```
/espi/1_1/resource/Batch/Bulk/{BulkID}
/espi/1_1/resource/Batch/Subscription/{SubscriptionID}
/espi/1_1/resource/Batch/Subscription/{SubscriptionID}/UsagePoint/{UsagePointID}
/espi/1_1/resource/Batch/BulkRetailCustomerInfo/{BulkID}
/espi/1_1/resource/Batch/RetailCustomer/{RetailCustomerID}
/espi/1_1/resource/Batch/BulkRetailDRPrgInfo/{BulkID}
/espi/1_1/resource/Batch/RetailDRPrgInfo/{RetailCustomerID}
```

`Bulk`-level resources take the `client_access_token`; customer-level resources
take that customer's `access_token`. Your `BulkID` is issued at approval. In the
harvested standard contract the Bulk download is `downloadBulkData`.

## Step 1 — request

`GET` the Batch resource with the date window in Zulu
(`published-min`/`published-max` or `updated-min`/`updated-max`) and a
`correlationID` you generate. Since Release 4.0 (2023-03-20) PG&E also honours an
`x-transaction` header label and returns it with a Correlation ID when the file
is ready — set both and store them against your job.

Expect an acknowledgement (`202`-style), not the payload. `204` means there is
no data in that range.

## Step 2 — receive the notification

PG&E POSTs to your registered Notification URI over HTTPS. The body is ESPI XML
carrying `ns0:BatchList` with an `ns0:resources` element containing the retrieval
URL(s). Three things can trigger a notification:

1. **async-batch-ready** — the ad hoc Batch request you made is prepared.
2. **daily-subscription-data-ready** — only if you ticked "Notify me as data is
   ready" at registration; PG&E packages the latest day's data plus corrections
   to the previous 24 hours. PG&E explicitly asks you NOT to enable this unless
   you retrieve daily.
3. **authorization-changed** — for example a customer cancellation. Re-read the
   Authorization resource and stop pulling that customer's data.

Match on the correlation ID; do not assume ordering.

## Step 3 — retrieve

`GET` each URL from the notification with the correct token class over mutual
TLS. Then acknowledge internally and move the job to done.

## Hard constraints

- There is no webhook signature, HMAC or replay-protection scheme documented.
  Treat the notification as an untrusted trigger: it tells you to go fetch, and
  the fetch is what is authenticated (mutual TLS plus bearer token). Never treat
  notification content as authoritative data.
- PG&E documents no retry policy or delivery guarantee for notifications. Keep
  your own job ledger and reconcile with a periodic Batch request.
- Rate limits still apply to the retrieval GETs: 1 request/second per vendor,
  2,000/hour and 20,000/24h per client ID, daily reset 5 p.m. Pacific.
- Notification URI, Redirect URI and Third-Party Portal URI are all set at
  registration and changed under Manage Registration — not through an API.

Full event catalog: `asyncapi/pge-share-my-data-notifications.yml`.
