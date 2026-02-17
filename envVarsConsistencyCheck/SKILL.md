---
name: envVarsConsistencyCheck
title: Mandatory and Optional Environment Variables Consistency Check
description: Validate consistency between application environment variable schema and Helm/Kubernetes configuration across multiple environments.
tags:
  - env
  - helm
  - kubernetes
  - devops
  - validation
  - ci
version: 1.0.0
---

# SKILL.md – Mandatory and Optional Environment Variables Consistency Check

## Purpose

Validate that all mandatory environment variables defined in the application code are correctly configured in Helm (`values.yaml`, `values-<env>.yaml`) and/or provided via CI/Kubernetes secrets.

Additionally, report the presence and effective values of optional environment variables across environments.

This skill is designed to produce deterministic, structured output suitable for CI logs, Merge Requests, and technical documentation.

---

## Scope

This skill applies when:

- The application defines environment variables in a schema file (e.g., `src/config.ts`).
- The deployment configuration uses:
  - `values.yaml` (default values)
  - `values-<env>.yaml` (environment overrides)
  - CI/Kubernetes secrets

Supported environments:

- dev
- int
- stg
- prod

---

## Definitions

- **Mandatory variables**: Variables listed in the `required` field of the schema.
- **Optional variables**: Variables defined in the schema but NOT listed in `required`.
- **Default value**: A value defined in `values.yaml` and not overridden.
- **Override value**: A value defined in `values-<env>.yaml`.
- **Secret variable**: A variable defined via CI/CD or Kubernetes secrets.

---

## Procedure

### 1. Extract Variables from Code

1. Locate the environment schema file (e.g., `src/config.ts`).
2. Extract:
   - All variables listed in the `required` field → **Mandatory variables**.
   - All other variables defined in the schema → **Optional variables**.

---

### 2. Resolve Configuration per Environment

For each environment (dev, int, stg, prod):

1. Start from `values.yaml` as the default configuration.
2. Apply overrides from `values-<env>.yaml` (if present).
3. Include variables defined as secrets (CI/Kubernetes).

Resolution rules:

- If defined in both default and environment-specific file → use environment value.
- If defined only in default → mark as `value (default)`.
- If defined as secret → mark as `(secret)`.
- If not defined anywhere → mark as `—`.

---

### 3. Validation Rules

- Every **mandatory variable** must be present in at least one of:
  - `values.yaml`
  - `values-<env>.yaml`
  - Secrets
- If missing for a specific environment → mark as `—`.
- Optional variables must always be included in the report, even if not defined.

No assumptions should be made beyond the provided configuration files.

---

## Output Requirements

The output MUST be:

- Deterministic
- Strictly structured
- Free of explanations, suggestions, or improvements
- Limited to the elements described below

### 1. Table – Mandatory Variables

- Rows: Mandatory variables
- Columns: dev, int, stg, prod
- Cell values must be one of:
  - `value`
  - `value (default)`
  - `(secret)`
  - `—`

Example:

**Mandatory variables:**

| Variable | dev | int | stg | prod |
|----------|-----|-----|-----|------|
| VAR1 | value | value | value (default) | value |
| VAR2 | (secret) | (secret) | (secret) | (secret) |
| VAR3 | — | — | — | — |

---

### 2. Table – Optional Variables

- Rows: Optional variables
- Columns: dev, int, stg, prod
- Same cell value rules as above

Example:

**Optional variables:**

| Variable | dev | int | stg | prod |
|----------|-----|-----|-----|------|
| OPT_VAR1 | value | — | value | — |
| OPT_VAR2 | — | — | — | — |

---

### 3. Missing Mandatory Variables List

Provide a final list of missing mandatory variables per environment.

Example format:

Missing mandatory variables:
- dev: VAR3
- int: VAR3
- stg: VAR3
- prod: VAR3

---

## Constraints

- Do NOT add commentary.
- Do NOT suggest improvements.
- Do NOT infer values not explicitly defined.
- Do NOT modify variable names.
- Use `—` exactly for missing values.
- Use `(secret)` exactly for secret-based values.
- Use `value (default)` exactly when the default value is used without override.

---

## Expected Outcome

This skill ensures:

- Reduced manual comparison effort.
- Structured and repeatable validation.
- CI-ready output.
- Standardized environment variable auditing across projects.

