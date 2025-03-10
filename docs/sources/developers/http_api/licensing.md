---
aliases:
  - /docs/grafana/latest/developers/http_api/licensing/
  - /docs/grafana/latest/http_api/licensing/
description: Enterprise Licensing HTTP API
keywords:
  - grafana
  - http
  - documentation
  - api
  - licensing
  - enterprise
title: 'Licensing HTTP API '
---

# Enterprise License API

Licensing is only available in Grafana Enterprise. Read more about [Grafana Enterprise]({{< relref "../../enterprise" >}}).

> If you are running Grafana Enterprise, for some endpoints you'll need to have specific permissions. Refer to [Role-based access control permissions]({{< relref "../../enterprise/access-control/custom-role-actions-scopes" >}}) for more information.

## Check license availability

> **Note:** Available in Grafana Enterprise v7.4+.

`GET /api/licensing/check`

Checks if a valid license is available.

**Required permissions**

See note in the [introduction]({{< ref "#enterprise-license-api" >}}) for an explanation.

| Action         | Scope |
| -------------- | ----- |
| licensing:read | n/a   |

### Examples

**Example request:**

```http
GET /api/licensing/check
Accept: application/json
Authorization: Bearer eyJrIjoiT0tTcG1pUlY2RnVKZTFVaDFsNFZXdE9ZWmNrMkZYbk
```

**Example response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 4

true
```

Status codes:

- **200** - OK

## Manually force license refresh

> **Note:** Available in Grafana Enterprise v7.4+.

`POST /api/licensing/token/renew`

Manually ask license issuer for a new token.

**Required permissions**

See note in the [introduction]({{< ref "#enterprise-license-api" >}}) for an explanation.

| Action           | Scope |
| ---------------- | ----- |
| licensing:update | n/a   |

### Examples

**Example request:**

```http
POST /api/licensing/token/renew
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJrIjoiT0tTcG1pUlY2RnVKZTFVaDFsNFZXdE9ZWmNrMkZYbk

{}
```

**Example response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 357

{
  "jti":"2",
  "iss":"https://grafana.com",
  "sub":"https://play.grafana.org/"
  "lid":"1",
  "included_admins":5,
  "included_viewers":10,
  "lic_exp_warn_days":30,
  "tok_exp_warn_days":2,
  "update_days":1,
  "prod":["grafana-enterprise"],
  "company":"Grafana Labs"
}
```

The response is a JSON blob available for debugging purposes. The
available fields may change at any time without any prior notice.

Status Codes:

- **200** - OK
- **401** - Unauthorized
- **403** - Access denied

## Remove license from database

> **Note:** Available in Grafana Enterprise v7.4+.

`DELETE /api/licensing/token`

Removes the license stored in the Grafana database.

**Required permissions**

See note in the [introduction]({{< ref "#enterprise-license-api" >}}) for an explanation.

| Action           | Scope |
| ---------------- | ----- |
| licensing:delete | n/a   |

### Examples

**Example request:**

```http
DELETE /api/licensing/token
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJrIjoiT0tTcG1pUlY2RnVKZTFVaDFsNFZXdE9ZWmNrMkZYbk

{"instance": "http://play.grafana.org/"}
```

JSON Body schema:

- **instance** – Root URL for the instance for which the license should be deleted. Required.

**Example response:**

```http
HTTP/1.1 202 Accepted
Content-Type: application/json
Content-Length: 2

{}
```

Status codes:

- **202** - Accepted, license removed or did not exist.
- **401** - Unauthorized
- **403** - Access denied
- **422** - Unprocessable entity, incorrect instance name provided.
