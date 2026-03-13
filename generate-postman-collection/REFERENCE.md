# Postman Collection v2.1 — Schema Reference

## Top-level structure

```json
{
  "info": {
    "name": "<collection name>",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "variable": [
    { "key": "baseUrl", "value": "http://localhost:3000", "type": "string" },
    { "key": "accessToken", "value": "", "type": "string" }
  ],
  "item": [ /* folders or request items */ ]
}
```

## Folder item

Used to group requests under a route prefix:

```json
{
  "name": "users",
  "item": [ /* request items */ ]
}
```

## Request item (GET — no body)

```json
{
  "name": "GET /healthcheck",
  "event": [
    {
      "listen": "test",
      "script": {
        "type": "text/javascript",
        "exec": [
          "pm.test(\"Status code is 200\", function () {",
          "  pm.response.to.have.status(200);",
          "});"
        ]
      }
    }
  ],
  "request": {
    "method": "GET",
    "header": [],
    "url": {
      "raw": "{{baseUrl}}/healthcheck",
      "host": ["{{baseUrl}}"],
      "path": ["healthcheck"]
    }
  },
  "response": []
}
```

## Request item (POST — JSON body)

```json
{
  "name": "POST /users",
  "event": [
    {
      "listen": "test",
      "script": {
        "type": "text/javascript",
        "exec": [
          "pm.test(\"Status code is 200\", function () {",
          "  pm.response.to.have.status(200);",
          "});"
        ]
      }
    }
  ],
  "request": {
    "method": "POST",
    "header": [
      { "key": "Content-Type", "value": "application/json" },
      { "key": "Authorization", "value": "Bearer {{accessToken}}" }
    ],
    "body": {
      "mode": "raw",
      "raw": "{\n  \"field1\": \"value1\",\n  \"field2\": \"value2\"\n}",
      "options": {
        "raw": { "language": "json" }
      }
    },
    "url": {
      "raw": "{{baseUrl}}/users",
      "host": ["{{baseUrl}}"],
      "path": ["users"]
    }
  },
  "response": [
    {
      "name": "200 — Success",
      "originalRequest": { /* copy of request above */ },
      "status": "OK",
      "code": 200,
      "_postman_previewlanguage": "json",
      "header": [{ "key": "Content-Type", "value": "application/json" }],
      "body": "{\n  \"message\": \"OK\"\n}"
    },
    {
      "name": "400 — Validation error",
      "originalRequest": {
        "method": "POST",
        "header": [{ "key": "Content-Type", "value": "application/json" }],
        "body": {
          "mode": "raw",
          "raw": "{}",
          "options": { "raw": { "language": "json" } }
        },
        "url": {
          "raw": "{{baseUrl}}/users",
          "host": ["{{baseUrl}}"],
          "path": ["users"]
        }
      },
      "status": "Bad Request",
      "code": 400,
      "_postman_previewlanguage": "json",
      "header": [{ "key": "Content-Type", "value": "application/json" }],
      "body": "{\n  \"statusCode\": 400,\n  \"error\": \"Bad Request\",\n  \"message\": \"body must have required property 'field1'\"\n}"
    }
  ]
}
```

## Path parameter in URL

```json
"url": {
  "raw": "{{baseUrl}}/users/:id",
  "host": ["{{baseUrl}}"],
  "path": ["users", ":id"],
  "variable": [
    { "key": "id", "value": "123", "description": "User ID" }
  ]
}
```

## Query parameters

```json
"url": {
  "raw": "{{baseUrl}}/users?page=1&limit=20",
  "host": ["{{baseUrl}}"],
  "path": ["users"],
  "query": [
    { "key": "page",  "value": "1"  },
    { "key": "limit", "value": "20" }
  ]
}
```

## Common custom headers

If the service uses custom propagated headers (e.g. correlation IDs, tracing), add them as variables and include them in requests:

```json
{ "key": "x-correlation-id", "value": "{{$guid}}", "description": "Unique request correlation ID" },
{ "key": "x-request-id",     "value": "{{$guid}}", "description": "Request identifier" }
```

Detect such headers by looking for middleware, hooks, or interceptors that read/write custom `x-*` headers.

## Body placeholder conventions

Derive realistic values from field names (not generic `"string"` / `0`):

| Field name pattern | Placeholder value |
|--------------------|-------------------|
| `*email*`          | `"user@example.com"` |
| `*name*`           | `"John Doe"` |
| `*phone*`          | `"+15550001234"` |
| `*id*`             | `"{{$guid}}"` |
| `*token*`          | `"{{accessToken}}"` |
| `*date*`           | today's ISO date |
| `*url*`            | `"https://example.com"` |
| boolean            | `true` |
| integer / number   | `1` |
| enum               | first enum value |
| required string    | `"<fieldName>-example"` |
