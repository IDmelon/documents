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

**Query parameters:**

- `limit` (optional) – Max number of items to return (e.g. `50`).

**Example:**

```http
GET https://skm.eu.idmelon.com/administrator/tokens/v2?limit=50
Authorization: YOUR_API_KEY
```

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

List and manage **security keys** in the workspace (active, pending, suspended, etc.).

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/devices` | List all security keys (devices) in the workspace |

**Query parameters:**

- `limit` (optional) – Max number of devices (e.g. `50`).

**Example:**

```http
GET https://skm.eu.idmelon.com/administrator/devices?limit=50
Authorization: YOUR_API_KEY
```

---

## Activity and authentication logs

Endpoints that correspond to **Activity** or **Authentication logs** in the Admin Panel (every authentication-related activity).

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/administrator/workspace/activity/logs` | Workspace activity / authentication logs |

**Query parameters:**

- `limit` (optional) – Max number of log entries (e.g. `50`).

**Example:**

```http
GET https://skm.eu.idmelon.com/administrator/workspace/activity/logs?limit=50
Authorization: YOUR_API_KEY
```

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
- Standard HTTP status codes apply (e.g. `200` OK, `401` Unauthorized for invalid or missing API key, `404` Not Found).
- Endpoints that accept `limit` may return paginated data; check the response for additional pagination fields if you need to request more results.

## Quick reference (Postman)

1. **Base URL:** Your SKM base URL (e.g. `https://skm.eu.idmelon.com`).
2. **Auth:** Type **API Key** → Key: `Authorization`, Value: `{your-api-key}`.
3. **Paths:** Use the endpoints above (e.g. `/administrator/statistics/users`, `/administrator/devices?limit=50`).

If you need more detail on a specific endpoint (request/response bodies or additional query parameters), refer to your Admin Panel behavior or contact IDmelon support.
