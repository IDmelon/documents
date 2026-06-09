---
title: "REST API"
description: "IDmelon Admin Panel REST API for workspace stats, users, security keys, and activity logs"
lead: ""
date: 2026-03-03T00:00:00+00:00
lastmod: 2026-03-03T00:00:00+00:00
draft: false
images: []
menu:
  docs:
    parent: "api_integrations"
weight: 10
toc: true
---

This document describes how to use the IDmelon Admin Panel REST API to access workspace data programmatically. The same data you see in the Admin Panel—statistics, users, user details, security keys, and authentication activity—can be retrieved via API for reporting, integrations, or automation.

## Base URL

All API requests are sent to your workspace **SKM (Security Key Manager) base URL**:

- **US (default):** `https://skm.idmelon.com`
- **EU:** `https://skm.eu.idmelon.com`
- **Dedicated deployment:** your own SKM host, normally `https://skm.idmelon.{companydomain}` (e.g. `https://skm.idmelon.acme.com`)

Use the base URL that corresponds to your Admin Panel / workspace and region.

## Authentication

The API uses **API key authentication**. Include your API key in every request.

### Using the API key

Send the API key in the **Authorization** header:

| Header       | Value            |
|-------------|-------------------|
| **Authorization** | Your API key (raw value) |

**Example (Postman):**

1. In the request **Authorization** tab, choose type **API Key**.
2. Set **Key** to `Authorization`.
3. Set **Value** to your API key.

**Example (cURL):**

```bash
curl -H "Authorization: YOUR_API_KEY" "https://skm.eu.idmelon.com/administrator/statistics/users"
```

**Example (HTTP header):**

```http
GET /administrator/statistics/users HTTP/1.1
Host: skm.eu.idmelon.com
Authorization: YOUR_API_KEY
```

> **Note:** Create and manage API keys in the Admin Panel under **Workspace > API Keys**. Keep your API key secret and do not share it or commit it to source control.

---

## Statistics

Endpoints that mirror the **Stats** area in the Admin Panel (workspace users, security keys, groups, and login activity).

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/statistics/users` | User-related statistics |
| GET | `/administrator/statistics/securitykeys` | Security key–related statistics |
| GET | `/administrator/statistics/groups` | Group-related statistics |
| GET | `/administrator/statistics/login` | Login / authentication statistics |

**Query parameters (e.g. login):**

- `timezone` (optional) – IANA timezone (e.g. `America/Vancouver`) for time-based stats.

**Example:**

```http
GET https://skm.eu.idmelon.com/administrator/statistics/login?timezone=America/Vancouver
Authorization: YOUR_API_KEY
```

---

## Users

List and work with users in the workspace.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users` | Current API context: returns the workspace and user info for the API key used. |
| GET | `/administrator/tokens/v2` | List all users in the workspace. |
| POST | `/administrator/tokens/deleteMany` | Delete one or more users (bulk delete). |

**Query parameters (list):**

- `limit` (optional) – Max number of items to return per page (e.g. `50`).
- `skip` (optional) – Number of items to skip (offset) for pagination (e.g. `50`).

**Example (list):**

```http
GET https://skm.eu.idmelon.com/administrator/tokens/v2?limit=50
Authorization: YOUR_API_KEY
```

The list response also includes `skip`, `limit`, `total`, and a `details` summary
(e.g. `activeTokens`, `pendingTokens`, `blockTokens`, `notInvitedTokens`,
`noDeviceTokens`) that mirrors the counters shown on the **All Users** page.

**Bulk delete users:**

`POST /administrator/tokens/deleteMany` removes one or more users. Send a JSON
body with a `tokens` array of user IDs (the `_id` values from the users list):

```http
POST https://skm.eu.idmelon.com/administrator/tokens/deleteMany
Authorization: YOUR_API_KEY
Content-Type: application/json

{ "tokens": ["6a0217b02571b6081b96e1e2"] }
```

A successful call returns:

```json
{ "message": "User has been successfully deleted." }
```

> **Warning:** This is a destructive, irreversible operation that deletes the
> user and their associated keys. Double-check the IDs before calling it.

---

## User details

Endpoints used for the **User Details** page: user info, activities, security keys, passkeys (credentials), and assigned groups.

Replace `{userId}` with the user’s ID (returned from the users list endpoint).

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/tokens/{userId}` | User details / info |
| GET | `/administrator/tokens/userInfo/{userId}/securitykeys/v2` | Security keys for the user |
| GET | `/administrator/tokens/{userId}/credentials` | Passkeys / credentials for the user |
| GET | `/administrator/tokens/{userId}/passwords` | Password-related data for the user |
| GET | `/administrator/groups` | Groups in the workspace; filter by user with `userId` |
| GET | `/administrator/tokens/{userId}/shared-with` | Entities this user’s tokens are shared with |
| GET | `/administrator/tokens/{userId}/shared-groups` | Groups with which this user’s tokens are shared |

**Query parameters:**

- `limit` (optional) – Max items (e.g. `50`) where supported.
- `userId` (optional) – For `/administrator/groups`, restricts results to groups for that user.

**Examples:**

```http
GET https://skm.eu.idmelon.com/administrator/tokens/USER_ID
Authorization: YOUR_API_KEY
```

```http
GET https://skm.eu.idmelon.com/administrator/tokens/userInfo/USER_ID/securitykeys/v2?limit=50
Authorization: YOUR_API_KEY
```

```http
GET https://skm.eu.idmelon.com/administrator/groups?userId=USER_ID
Authorization: YOUR_API_KEY
```

---

## Security keys

The **Security Keys** page lists every security key (credential) issued in the
workspace. Each key belongs to a user (`token.owner`) and tracks its usage
(`lastActivity` and `totalActivities`). This is the endpoint to use for key
inventory, lifecycle, and activity reporting.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/securityDevice` | List security keys, with filtering, sorting, and pagination |

**Query parameters:**

| Parameter | Description | Example |
|-----------|-------------|---------|
| `status` | Filter by key lifecycle status. `20` = active keys; other values map to the other tabs of the Security Keys page (e.g. `30` = pending/registered). | `status=20` |
| `sort` | Sort the results as `field,direction`, where direction is `ascend` or `descend` (note: not `asc`/`desc`). `lastActivity` is the supported sort field. | `sort=lastActivity,ascend` |
| `limit` | Maximum number of keys to return per page. | `limit=50` |
| `skip` | Number of keys to skip (offset) for pagination. | `skip=50` |

**Selected response fields (per key):**

| Field | Meaning |
|-------|---------|
| `name` | Friendly name of the key |
| `token.owner` | Email of the user the key belongs to |
| `token.name` | Full name of the owning user |
| `status` | Lifecycle status (e.g. `20` = active) |
| `type` | Key form factor (e.g. phone, badge/card) |
| `lastActivity.createdAt` | Timestamp of the key's most recent authentication |
| `totalActivities` | Lifetime count of authentications for this key |
| `hasOfflineAccess`, `proximity`, `verification`, `isBlocked` | Key capability / policy flags |

The response envelope also includes `skip`, `limit`, a `statistics` breakdown
(counts by key type), and a `filters` echo of the filters that were applied.

### Useful queries

#### Find the least-active (stale) security keys

Sort ascending by `lastActivity` to surface the keys that have gone the longest
without being used — ideal for spotting dormant keys that may be candidates for
revocation or cleanup:

```http
GET https://skm.eu.idmelon.com/administrator/securityDevice?status=20&limit=50&sort=lastActivity,ascend
Authorization: YOUR_API_KEY
```

```bash
curl -H "Authorization: YOUR_API_KEY" \
  "https://skm.eu.idmelon.com/administrator/securityDevice?status=20&limit=50&sort=lastActivity,ascend"
```

#### Find the most-recently-used keys

Reverse the direction to list the most active keys first:

```http
GET https://skm.eu.idmelon.com/administrator/securityDevice?status=20&limit=50&sort=lastActivity,descend
Authorization: YOUR_API_KEY
```

#### List only active keys (no sorting)

```http
GET https://skm.eu.idmelon.com/administrator/securityDevice?status=20&limit=50
Authorization: YOUR_API_KEY
```

#### List pending / non-active keys

```http
GET https://skm.eu.idmelon.com/administrator/securityDevice?status=30&limit=50
Authorization: YOUR_API_KEY
```

> **Tip:** Combine `sort=lastActivity,ascend` with a high `limit` to build a
> "stale key" report, then page through with `skip` for large workspaces.

### Delete security keys

Remove one or more security keys from the workspace. Deleting a key revokes it
and frees any license associated with it.

| Method | Endpoint | Description |
|--------|----------|-------------|
| DELETE | `/administrator/securityDevice/{keyId}` | Delete a single security key by its `_id` |
| DELETE | `/administrator/securityDevice/` | Delete multiple security keys (bulk) |

#### Delete a single key

Use the key's `_id` (the `_id` field from the `/administrator/securityDevice`
list). The same value is sent both in the path and as the `_id` query parameter:

```http
DELETE https://skm.eu.idmelon.com/administrator/securityDevice/KEY_ID?_id=KEY_ID
Authorization: YOUR_API_KEY
```

```bash
curl -X DELETE -H "Authorization: YOUR_API_KEY" \
  "https://skm.eu.idmelon.com/administrator/securityDevice/KEY_ID?_id=KEY_ID"
```

Success response:

```json
{ "message": "The user's security key has been successfully deleted." }
```

#### Delete multiple keys (bulk)

Send a JSON body with an `_ids` array of key `_id` values to the collection path
(note the trailing slash):

```http
DELETE https://skm.eu.idmelon.com/administrator/securityDevice/
Authorization: YOUR_API_KEY
Content-Type: application/json

{ "_ids": ["65dcff77a5f39100085fe897"] }
```

Success response:

```json
{ "message": "The selected security keys have been successfully deleted." }
```

> **Warning:** Deleting a security key is irreversible — the user must enroll a
> new key to authenticate again. Deletion removes only the key; to deprovision
> the **user**, manage the user lifecycle through your source directory
> (e.g. Microsoft Entra ID via SCIM) or the user-level
> [`/administrator/tokens/deleteMany`](#users) endpoint.

#### Automate inactive-key cleanup

Combine the listing, sorting, and deletion endpoints to remove keys that have
been unused for a defined period:

1. **List & sort by activity** — `GET /administrator/securityDevice?status=20&limit=50&sort=lastActivity,ascend`
   returns active keys with the least recently used first.
2. **Select stale keys** — keep entries whose `lastActivity.createdAt` is older
   than your inactivity threshold (X days). Keys that have never been used have
   no `lastActivity` and `totalActivities: 0`.
3. **Delete** — call `DELETE /administrator/securityDevice/` with the selected
   `_id` values in `_ids` to revoke them and reclaim their licenses.

Run this as a scheduled job to continuously enforce an inactivity policy.

---

## Devices (end-user apps)

Distinct from security keys, the **Devices** endpoint lists the end-user devices
(phones and desktops running the IDmelon app) registered in the workspace.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/devices` | List end-user devices (IDmelon app installations) |

Each entry includes `displayName`, `createdDateTime`, `hasOfflineAccess`, and,
where available, an `apps` object with the installed app `version`. The response
envelope includes `skip`, `limit`, `total`, `totalOfDevices`, `totalOfAccessKey`,
and `totalWithoutApp`.

**Query parameters:**

- `limit` (optional) – Max number of devices per page (e.g. `50`).
- `skip` (optional) – Offset for pagination.

**Example:**

```http
GET https://skm.eu.idmelon.com/administrator/devices?limit=50
Authorization: YOUR_API_KEY
```

---

## Authentication logs

This endpoint backs the **Reports → Authentication Logs** page. It returns every
authentication-related event in the workspace — each time a user authenticates to
a relying party (application/site) with a security key. Workspaces commonly
accumulate tens of thousands of entries, so the endpoint supports filtering by
action, relying party, and date range, plus pagination.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/workspace/activity/logs` | List authentication / activity log entries |

**Query parameters:**

| Parameter | Description | Example |
|-----------|-------------|---------|
| `action` | Filter by activity type. `2` = authentication (login) events, which make up the bulk of the logs; other values (e.g. `1`, `4`) correspond to the other entries in the Authentication Logs **action** dropdown. | `action=2` |
| `rp` | Filter by relying party (the application or site the user signed in to). Matches the relying party name or domain (e.g. `Paypal` matches `PayPal` / `paypal.com`). | `rp=idmelon.com` |
| `from` | Start of the date range (inclusive), ISO 8601 UTC. Filters on the event timestamp. | `from=2026-06-01T07:00:00.000Z` |
| `to` | End of the date range (inclusive), ISO 8601 UTC. | `to=2026-06-08T07:00:00.000Z` |
| `limit` | Maximum number of log entries to return per page. | `limit=50` |
| `skip` | Number of entries to skip (offset) for pagination. | `skip=50` |

**Response:**

The response is an envelope with the matching entries plus pagination metadata:

```json
{
  "data": [
    {
      "createdAt": "2026-06-08T23:21:26.614Z",
      "action": 2,
      "status": 1,
      "rpInfo": { "name": "IDmelon", "id": "idmelon.com" },
      "userInfo": { "name": "user@example.com", "displayName": "user@example.com" },
      "ownerId": "user@example.com",
      "deviceName": "User's iPhone",
      "securityKeyDevice": "642dd340d45192000bc6bdb8"
    }
  ],
  "total": 23833,
  "skip": 0,
  "limit": 50
}
```

**Selected fields (per log entry):**

| Field | Meaning |
|-------|---------|
| `createdAt` | Timestamp of the event |
| `action` | Activity type (e.g. `2` = authentication) |
| `status` | Result status of the event (e.g. `1` = success) |
| `rpInfo.name` / `rpInfo.id` | Relying party the user authenticated to (name and domain) |
| `userInfo.name` / `userInfo.displayName` | The user who authenticated |
| `ownerId` | Owner email of the security key |
| `deviceName` | Friendly name of the security key used |
| `securityKeyDevice` | The `_id` of the security key used — matches `_id` from `/administrator/securityDevice` |
| `credentialId` | The credential (passkey) identifier |
| `deletedDevice` | `true` if the key has since been deleted |

> **Cross-reference:** A log entry's `securityKeyDevice` and `_id` line up with
> the `lastActivity` recorded on `/administrator/securityDevice`, so you can pivot
> from a key to its authentication history and back.

### Useful queries

#### Recent authentications only

```http
GET https://skm.eu.idmelon.com/administrator/workspace/activity/logs?limit=50&action=2
Authorization: YOUR_API_KEY
```

#### Logins to a specific application (relying party)

```http
GET https://skm.eu.idmelon.com/administrator/workspace/activity/logs?limit=50&rp=idmelon.com
Authorization: YOUR_API_KEY
```

#### Activity for a relying party within a date range

Combine `rp` with `from`/`to` to build a per-application audit report for a
reporting period:

```http
GET https://skm.eu.idmelon.com/administrator/workspace/activity/logs?limit=50&rp=idmelon.com&from=2026-06-01T07:00:00.000Z&to=2026-06-08T07:00:00.000Z
Authorization: YOUR_API_KEY
```

```bash
curl -H "Authorization: YOUR_API_KEY" \
  "https://skm.eu.idmelon.com/administrator/workspace/activity/logs?limit=50&from=2026-06-01T07:00:00.000Z&to=2026-06-08T07:00:00.000Z"
```

#### Page through the full log

Use `total` from the first response to page with `skip` (`0`, `50`, `100`, …):

```http
GET https://skm.eu.idmelon.com/administrator/workspace/activity/logs?limit=50&skip=50
Authorization: YOUR_API_KEY
```

> **Tip:** Logs are returned newest-first. Pair `from`/`to` with `limit` and
> `skip` to export a bounded time window without walking the entire history.

---

## Other workspace endpoints

These endpoints support other Admin Panel areas (workspace settings, admins, API keys, groups, etc.):

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/ribbon` | UI ribbon data (notifications, state) |
| GET | `/administrator/organization-units/names` | List organization unit names |
| GET | `/administrator/organization-units/names?hasAdmin=true` | Organization unit names (admin filter) |
| GET | `/administrator/organization-units` | List organization units |
| GET | `/administrator/detials` | Workspace details (administrator context) |
| GET | `/administrator/workspace/administrators` | Workspace administrators |
| GET | `/administrator/workspace/api-keys` | Workspace API keys (management) |
| GET | `/administrator/securityDevice/self-service` | Self-service security device settings |
| GET | `/administrator/workflows` | Workflows (e.g. `limit=50`) |
| GET | `/administrator/bills/current` | Current billing info |
| GET | `/administrator/bills/billingPolicy` | Billing policy |

Use the same **Authorization** header with your API key for all of these.

---

## Response format

- Successful responses return **JSON**.
- Standard HTTP status codes apply (e.g. `200` OK, `304` Not Modified for cached/conditional requests, `401` Unauthorized for invalid or missing API key, `404` Not Found).
- **Pagination:** list endpoints accept `limit` (page size) and `skip` (offset),
  and echo them back in the response alongside a `total` count. To page through
  results, increase `skip` by `limit` on each request (e.g. `skip=0`, `skip=50`,
  `skip=100`, …) until you have read `total` items.
- **Sorting:** where supported (e.g. `/administrator/securityDevice`), use
  `sort=field,direction` with direction `ascend` or `descend`.

## Quick reference (Postman)

1. **Base URL:** Your SKM base URL (e.g. `https://skm.eu.idmelon.com`).
2. **Auth:** Type **API Key** → Key: `Authorization`, Value: `{your-api-key}`.
3. **Paths:** Use the endpoints above (e.g. `/administrator/statistics/users`, `/administrator/securityDevice?status=20&limit=50&sort=lastActivity,ascend`).

If you need more detail on a specific endpoint (request/response bodies or additional query parameters), refer to your Admin Panel behavior or contact IDmelon support.
