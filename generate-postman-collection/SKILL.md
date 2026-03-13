---
name: generate-postman-collection
description: Generate a Postman Collection v2.1 JSON file by analysing the current codebase. Identifies language, framework, all exposed endpoints and their input schemas, then produces a ready-to-import collection with realistic request examples for each endpoint. Use when the user asks to "create a Postman collection", "generate postman", "export endpoints to postman", or "create API collection".
---

# Generate Postman Collection

## Workflow

### Step 1 — Analyse the repo

Explore the codebase to identify:

- **Language**: TypeScript, Python, Java, Go, …
- **Framework**: Fastify, Express, NestJS, FastAPI, Spring Boot, …
- **All exposed endpoints**: HTTP method + path + route prefix
- **Input schemas** for each endpoint: request body, query params, path params, headers

Discovery strategy by framework:

| Framework | Where to look |
|-----------|--------------|
| Fastify | `routes.ts` / `app.ts` → registered plugins → handler files + `schema.ts` siblings |
| Express / NestJS | Controller files, `@Get`, `@Post`, `router.*` decorators/calls |
| FastAPI | `@app.get`, `@app.post`, Pydantic models |
| Spring Boot | `@RestController`, `@RequestMapping`, `@GetMapping`, … |
| OpenAPI spec | If `openapi.yaml` / `swagger.json` exists, prefer that as the source of truth |

**If identification fails**: if after exploration you cannot determine the framework, or cannot find any endpoints, **stop and ask the user** before continuing. Example questions:
- *"I couldn't identify the framework. Is this a [Fastify / Express / FastAPI / other] project?"*
- *"I found the framework but couldn't locate the route definitions. Can you point me to the file or folder where routes are declared?"*
- *"I found these endpoints but I'm not confident they're complete. Please review and tell me if any are missing."*

Do **not** guess or generate a collection with placeholder routes. Always confirm with the user when uncertain.

### Step 2 — Print a summary for user review

After discovery, print **exactly** this format and ask for confirmation before proceeding:

```
- Language: <language>
- Framework: <framework>

Endpoints:
- <METHOD> <full-path>        [<short description if available>]
- <METHOD> <full-path>        [<short description if available>]
…
```

Ask: *"Is this correct? Should I add, remove, or adjust anything before generating the collection?"*

Do **not** proceed to step 3 until the user confirms.

### Step 3 — Generate the collection

Build a Postman Collection v2.1 JSON following the schema in [REFERENCE.md](REFERENCE.md).

Rules:
- Collection `name`: derive from `package.json` `name`, `pyproject.toml`, or the repo folder name.
- Use `{{baseUrl}}` as the host variable. Add it to `collection.variable`.
- Add `{{accessToken}}` as an auth variable if any endpoint requires a bearer token (look for auth hooks, middleware, or `Authorization` header references).
- **Group endpoints into folders** by route prefix (e.g. `/users`, `/products`).
- For every endpoint, create **at least one named example** (happy path). Add extra named examples for:
  - Validation error (400) — send a body with a missing required field.
  - Unauthorised (401) — if auth is required.
  - Not found (404) — for endpoints with path params.
- Populate request bodies with **realistic placeholder values** derived from the schema field names and types (not just `"string"` / `0`).
- Add a simple **test script** on every item that asserts the expected status code with `pm.test(...)`.
- Save to the repo root.

Output filename: `<collection-name>.postman_collection.json`

### Step 4 — Create the file

Write the generated JSON to disk and confirm the file path to the user.
