---
name: PG&E Share My Data — onboard, connect and get authorized
description: >-
  Take a registered third party from client credentials to a live customer
  authorization on PG&E Share My Data — client access token, connectivity test,
  the ESPI OAuth authorization-code flow, and reading back the granted scope.
api: openapi/green-button-alliance-espi-openapi.json
provider: Pacific Gas and Electric
generated: '2026-07-27'
method: generated
source: >-
  Grounded in operationIds verified in
  openapi/green-button-alliance-espi-openapi.json (the Green Button Alliance
  contract for the ESPI standard PG&E implements) and in PG&E's own published
  endpoints, token classes and flows from
  https://www.pge.com/en/save-energy-and-money/energy-saving-programs/smartmeter/third-party-companies.html
operations:
  - findApplicationInformations
  - getApplicationInformation
  - findAuthorizations
  - getAuthorization
---

# Onboard and authorize on PG&E Share My Data

## Before you start — this API is not self-serve

You cannot obtain credentials programmatically. PG&E requires, in order: a
9-digit US Employer Identification Number, registration at
https://sharemydata.pge.com/, eligibility screening against California Public
Utilities Commission standing, acceptance of the Share My Data Platform Terms of
Use and the CPUC Customer Data Access Tariff, and a CA-issued TLS 1.2 SHA-2
X.509 client certificate with an RSA key of at least 2048 bits (self-signed is
rejected). You must then pass the connectivity, OAuth and ApplicationInformation
tests within 90 days or the registration expires. Only then are production
`client_id` / `client_secret` issued. If you do not have these, stop — there is
no sandbox key.

## Transport rule that applies to every step

Every request except the browser redirects in the authorization-code flow runs
over mutual (two-way) TLS 1.2 with your client certificate attached. An
anonymous or certificate-less call returns `HTTP 400 Invalid Certificate` from
the gateway. See `authentication/pge-authentication.yml`.

## Step 1 — get a client access token

```
POST https://api.pge.com/datacustodian/oauth/v2/token
     ?grant_type=client_credentials
Authorization: Basic base64(client_ID:client_Secret)
```

Test-environment equivalent:
`https://api.pge.com/datacustodian/test/oauth/v2/token`.

The response is an XML `Response` document with `client_access_token`,
`expires_in` (3600), `scope` and `token_type` (Bearer). Cache it for its hour;
use its refresh token (1 year) to rotate.

## Step 2 — confirm connectivity

```
GET https://api.pge.com/GreenButtonConnect/espi/1_1/resource/ReadServiceStatus
Authorization: Bearer {client_access_token}
```

Expect a `ServiceStatus` document with `currentStatus` of `1`. This is PG&E's
only health signal — there is no public status page.

## Step 3 — read your own application record

Call `getApplicationInformation` (PG&E path
`/espi/1_1/resource/ApplicationInformation/{ApplicationInformationID}`) with the
`registration_access_token` — NOT the client access token. `findApplicationInformations`
returns the feed form. This confirms your redirect URIs, notification URI and
registered scope.

## Step 4 — send the customer through the authorization-code flow

Redirect the customer's browser to PG&E's production authorization endpoint:

```
GET https://sharemydata.pge.com/myAuthorization
    ?client_id={client_id}
    &redirect_uri={registered_redirect_uri}
    &response_type=code
    &state={optional_state}
```

During testing use `https://api.pge.com/datacustodian/test/oauth/v2/authorize`
instead. The customer authenticates on PG&E's site (or the Guest Access tab) and
chooses data groups, interval duration and authorization period. PG&E redirects
back to your `redirect_uri` with `authorization_code`, the granted `scope`
string and your `state`. The code expires in 600 seconds.

## Step 5 — exchange the code

```
POST https://api.pge.com/datacustodian/oauth/v2/token
     ?grant_type=authorization_code&code={code}&redirect_uri={redirect_uri}
Authorization: Basic base64(client_ID:client_Secret)
```

Attach your SSL certificate. You get `access_token` (1 hour), `refresh_token`
(1 year), the granted `scope`, `resourceURI` and `authorizationURI`.

## Step 6 — read the granted scope back

Call `findAuthorizations` / `getAuthorization` (PG&E path
`/espi/1_1/resource/Authorization/{AuthorizationID}`) with the
`client_access_token` to confirm what the customer actually granted. The scope
is an ESPI function-block string, for example:

```
FB=1_3_8_13_14_18_19_31_32_35_37_38_39_4_5_15;AdditionalScope=Usage;IntervalDuration=900_3600;BlockDuration=Daily;dataCustodianId=PGE
```

Parse it, do not assume it. `FB=4` means interval usage was authorized, `FB=16`
means billing with cost, `FB=46`/`FB=47` mean customer/account/program-enrollment
data. The full table is in `scopes/pge-scopes.yml`.

## Step 7 — revoke when you are done

`DELETE /espi/1_1/resource/Authorization/{AuthorizationID}` with the
`client_access_token`. Customers can also cancel from their PG&E dashboard, and
PG&E will POST a notification to your registered Notification URI when an
authorization changes.

## Rules

- Never call an endpoint without the client certificate.
- Use the right token class per resource: `registration_access_token` for
  ApplicationInformation, `client_access_token` for third-party-level and Bulk
  resources, the customer `access_token` for Subscription resources. Wrong class
  yields `403`.
- Stay under 1 request/second per vendor, 2,000/hour and 20,000/24h per client
  ID (daily reset 5 p.m. Pacific). See `rate-limits/pge-rate-limits.yml`.
- Repoint from the test endpoints to production after testing — PG&E states this
  explicitly.
