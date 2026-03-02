---
name: env-vars-consistency-check
title: Mandatory and Optional Environment Variables Consistency Check
description: Validate consistency between application environment variable schema and Helm/Kubernetes configuration across multiple environments, resolving real values, detecting CI/Kubernetes secrets, and identifying code-level defaults.
tags:
  - env
  - helm
  - kubernetes
  - devops
  - validation
  - ci
version: 1.3.0
---

# Purpose

Validate that all mandatory environment variables defined in the application code are correctly configured in Helm (`values.yaml`, `values-<env>.yaml`) and/or provided via CI/Kubernetes secrets.

Additionally, report:

- The **actual resolved values** per environment
- Variables sourced from **CI/Kubernetes secrets**
- **Default values defined in code**, including both schema-level defaults and runtime fallbacks

This skill produces deterministic, structured output suitable for CI logs, Merge Requests, and technical documentation.

---

# Scope

This skill applies when:

- Environment variables are defined in a schema file (e.g., `src/config.ts`)
- Defaults may be defined in schema or directly in code logic (`??`, `||`, `if`)
- Deployment uses `values.yaml`, `values-<env>.yaml`, CI/CD variables, or Kubernetes secrets

Supported environments:

- dev
- int
- stg
- prod

---

# Definitions

- **Mandatory variables**: Variables listed in the `required` field of the schema.
- **Optional variables**: Variables defined in the schema but not listed in `required`.
- **Helm default**: Value defined in `values.yaml`.
- **Environment override**: Value defined in `values-<env>.yaml`.
- **Secret variable**: Variable defined in CI/CD or Kubernetes secrets.
- **Code-level default**: Fallback defined in code (schema default, `??`, `||`, `if` logic).
- **Resolved value**: Effective runtime value after applying resolution priority.

---

# Procedure

## 1. Extract Variables from Code

1. Locate the schema file (e.g., `src/config.ts`).
2. Extract mandatory variables from `required`.
3. Extract optional variables from schema definitions.

## 2. Detect Code-Level Defaults

Identify defaults defined in:

- **Schema default**: `z.string().default("3000")`
- **Nullish coalescing (`??`)**: `process.env.PORT ?? "3000"`
- **Logical OR (`||`)**: `process.env.HOST || "localhost"`
- **Conditional fallback**:

    if (!timeout) { timeout = "5000"; }

Rules:

- Record explicit fallback values as code-default.
- Apply only if not provided by Helm or secret.

## 3. Detect Secret Variables

- Mark variable as `secret` if defined in CI/CD or Kubernetes Secret.
- Never expose secret values.

## 4. Resolve Configuration per Environment

Apply resolution priority:

1. Secret
2. Environment override
3. Helm default
4. Code-level default
5. Missing

Resolution rules:

- Secret → `secret`
- Environment override → actual value
- Helm default → `value (helm-default)`
- Code default → `value (code-default)`
- Not defined → `—`

---

# Validation Rules

- Mandatory variables must resolve via Helm, secret, or code-default.
- Optional variables must always be included in output.
- Do not infer defaults beyond explicit code logic.

---

# Output Requirements

- Deterministic
- Strictly structured
- No commentary
- Limited to defined tables and missing list

## Table – Mandatory Variables

| Variable | dev | int | stg | prod |
|----------|-----|-----|-----|------|
| Example | value | value | value (helm-default) | value |
| SecretVar | secret | secret | secret | secret |
| CodeDefaultVar | 3000 (code-default) | 3000 (code-default) | 8080 | 8080 |
| MissingVar | — | — | — | — |

## Table – Optional Variables

| Variable | dev | int | stg | prod |
|----------|-----|-----|-----|------|
| OptionalExample | debug | — | info | — |
| Timeout | 5000 (code-default) | 5000 (code-default) | 5000 (code-default) | 5000 (code-default) |
| OptionalSecret | secret | secret | — | — |

## Missing Mandatory Variables List

Provide a final list of mandatory variables that resolve to `—` per environment.

Example:

- dev: MissingVar
- int: MissingVar
- stg: MissingVar
- prod: MissingVar

---

# Constraints

- Do not add commentary
- Do not suggest improvements
- Do not expose secret values
- Do not modify variable names
- Use exact tokens: `secret`, `(helm-default)`, `(code-default)`, `—`

---

# Expected Outcome

- Accurate detection of Helm defaults and code-level defaults
- Correct recognition of CI/Kubernetes secrets
- Deterministic and reproducible CI-ready validation output
- Standardized environment variable auditing across projects
