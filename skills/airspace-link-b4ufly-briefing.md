---
name: Get a B4UFLY airspace briefing
description: Authenticate to the AirHub API and retrieve a B4UFLY airspace briefing for a point location or an area geometry.
api: https://developers.airspacelink.com/
operations:
  - api-31492994 Get OAuth Token
  - api-29744368 B4UFLY Briefing By Location (v2)
  - api-29744367 B4UFLY Briefing By Area (v2)
scopes:
  - briefing:location:read
  - briefing:area:read
---

# Get a B4UFLY airspace briefing

Use the AirHub API to check whether a drone flight location or area is clear per FAA B4UFLY data.

## Prerequisites
- A `client_id`, `client_secret`, and `x-api-key` supplied by Airspace Link.
- Choose an environment by base URL: sandbox `https://airhub-api-sandbox.airspacelink.com` or live `https://airhub-api.airspacelink.com`. The base URL alone selects live vs sandbox mode.

## Steps
1. **Get an access token.** `POST {base}/v1/oauth/token` with `Content-Type: application/x-www-form-urlencoded` and body `grant_type=client_credentials&client_id=...&client_secret=...&scope=briefing:location:read briefing:area:read`. Read the token from `data.accessToken` in the response envelope.
2. **Call the briefing.** Send every request with `Authorization: Bearer <accessToken>` AND the `x-api-key` header. For a point, call the B4UFLY Briefing By Location (v2) operation; for a polygon, call B4UFLY Briefing By Area (v2).
3. **Read the envelope.** Responses are wrapped as `{ data, message, statusCode }` — the briefing payload is under `data`.

## Error handling
- `400` malformed request, `403` missing scope / permission denied, `404` not found, `500` server error — all returned in the standard envelope. See `errors/airspace-link-problem-types.yml`.
