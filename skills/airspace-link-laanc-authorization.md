---
name: Submit and track a LAANC authorization
description: Create a LAANC operation and submit, retrieve, and cancel an FAA LAANC authorization through the AirHub API.
api: https://developers.airspacelink.com/
operations:
  - api-31492994 Get OAuth Token
  - api-10829372 Create LAANC Operation
  - api-29742696 Submit LAANC Authorization
  - api-29742697 Get LAANC Authorization
  - api-29742698 Cancel LAANC Authorization
scopes:
  - laanc:create
  - laanc:read
  - laanc:update
---

# Submit and track a LAANC authorization

Use the AirHub LAANC SDSP surface to obtain FAA authorization for a drone operation in controlled airspace.

## Prerequisites
- `client_id`, `client_secret`, and `x-api-key` from Airspace Link.
- Base URL selects the environment (sandbox `https://airhub-api-sandbox.airspacelink.com`, live `https://airhub-api.airspacelink.com`).

## Steps
1. **Get an access token.** `POST {base}/v1/oauth/token` (client-credentials, form-urlencoded) requesting `scope=laanc:create laanc:read laanc:update`. Read `data.accessToken`.
2. **Create the LAANC operation** (Create LAANC Operation, scope `laanc:create`) to define the flight geometry, altitude, and timing.
3. **Submit the authorization** (Submit LAANC Authorization, scope `laanc:create`).
4. **Poll status** (Get LAANC Authorization, scope `laanc:read`) — review `authorizations[].status` and any `notices[]` (rescinded/invalid actions).
5. **Cancel if needed** (Cancel LAANC Authorization, scope `laanc:update`).

## Rules
- Every request needs both `Authorization: Bearer <token>` and `x-api-key`.
- Responses use the `{ data, message, statusCode }` envelope; handle `403` (scope missing) and `404` (unknown operation id) per `errors/airspace-link-problem-types.yml`.
