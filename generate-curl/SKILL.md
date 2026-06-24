---
name: generate-curl
description: Generates a curl command for a given service endpoint. By default produces a shell-ready terminal curl with shell variable placeholders (e.g. $BASE_URL, $AUTH_TOKEN). When the user explicitly asks for Postman, produces a Postman-compatible curl using built-in variables (e.g. {{$guid}}) and environment variables (e.g. {{baseUrl}}). Use when asked to generate a curl, create a curl for an endpoint, build a Postman request, or produce an example HTTP call. Do not use for generating production HTTP clients or writing tests.
---

# generate-curl

## Procedures

**Step 1: Determine output mode**
- Default to mode = `terminal`.
- Switch to mode = `postman` only if the user explicitly requests Postman (e.g. "for Postman", "Postman-compatible", "import in Postman").

**Step 2: Discover endpoint details**
1. If the endpoint is not fully specified, inspect the codebase to locate the route handler and infer: HTTP method, full path, required headers, path/query parameters, and request body shape.
2. Classify each field as static or dynamic (IDs, tokens, timestamps, UUIDs, etc.).

**Step 3: Map dynamic values**

_Terminal mode_
- Use `$UPPER_SNAKE_CASE` shell variable syntax for every dynamic value (e.g. `$BASE_URL`, `$AUTH_TOKEN`, `$USER_ID`).
- Do NOT use Postman `{{...}}` syntax.

_Postman mode_
- Read `references/postman-dynamic-variables.md` to pick the correct built-in for each dynamic field:
  - UUID / GUID → `{{$guid}}`, Unix timestamp → `{{$timestamp}}`, ISO timestamp → `{{$isoTimestamp}}`, random int → `{{$randomInt}}`, email → `{{$randomEmail}}`, name → `{{$randomFullName}}`, IP → `{{$randomIp}}`, boolean → `{{$randomBoolean}}`.
- Map infrastructure values (base URL, auth token, tenant IDs) to `{{camelCase}}` environment variables: `{{baseUrl}}`, `{{authToken}}`.
- Correlation / trace ID headers → `{{$guid}}`.

**Step 4: Build the curl command**

Shared rules (both modes):
- Include `--location --request <METHOD>`.
- One `--header` flag per header, each line ending with ` \`.
- Use `--data-raw` for JSON bodies; omit for GET/HEAD/DELETE with no body.
- Do NOT include real secrets or tokens.
- For multipart payloads, replace `--data-raw` with `--form` and remove the JSON Content-Type header.

_Terminal mode_ template:
```sh
curl --location --request <METHOD> "${BASE_URL}<path>" \
  --header 'Content-Type: application/json' \
  --header '<HeaderName>: $<VAR_NAME>' \
  --data-raw '{
    "<field>": "$<VAR_NAME>"
  }'
```
Use double quotes around the URL to allow shell variable expansion.

_Postman mode_ template:
```
curl --location --request <METHOD> '{{baseUrl}}<path>' \
  --header 'Content-Type: application/json' \
  --header '<HeaderName>: {{variable}}' \
  --data-raw '{
    "<field>": "{{variable}}"
  }'
```
Use single quotes around the URL so Postman resolves `{{...}}` at runtime. Do NOT escape the delimiters.

**Step 5: Annotate the output**

_Terminal mode_ — list every shell variable and how to export it:
```sh
export BASE_URL="https://api.example.com"
export AUTH_TOKEN="your-token-here"
```

_Postman mode_ — provide a markdown table of every variable used:

| Variable | Type | Purpose |
|---|---|---|
| `{{baseUrl}}` | Environment | Service base URL |
| `{{authToken}}` | Environment | Bearer auth token |
| `{{$guid}}` | Built-in | Auto-generated unique ID |

## Error Handling
- If the endpoint cannot be found in the codebase, ask the user to provide the HTTP method, path, required headers, and body schema before proceeding.
- If a field has no matching Postman built-in (Postman mode), use a descriptive `{{camelCase}}` environment variable and mark it "Environment – set manually" in the table.
- If the request requires multipart/form-data, switch `--data-raw` to `--form` and remove the `Content-Type: application/json` header.
