---
name: envVars
title: Mandatory and Optional Environment Variables Check
description: Skill to be used validate the coverage of required and optional environment variables between code and Helm/K8s configurations
tags:
  - env
  - helm
  - k8s
  - validation
  - ci
version: 1.0.0
---


# SKILL.md - Mandatory and Optional Environment Variables Check

## Purpose
Verify that all mandatory environment variables required by the code (e.g., validation schema in config.ts) are actually defined in the environment configuration files (Helm values*.yaml, secrets, pipeline). Also, report the presence and values of optional environment variables.


## Procedure

1. **Extract mandatory and optional variables**
   - Locate the environment validation file (e.g., src/config.ts).
   - List all variables in the `required` array of the schema as mandatory.
   - List all other variables in the schema as optional (those not in `required`).

2. **Extract defined variables**
   - For each values.yaml, values-<env>.yaml file:
     - List all keys under `envVars`.
     - Treat variables in values.yaml as defaults for all environments.
     - Override with environment-specific values if present.
   - For secrets:
     - List variables defined as secrets (e.g., .gitlab-ci.yml, KUBERNETES_SECRETS).

3. **Comparison**
   - For each mandatory variable, check if it is present in at least one of:
     - envVars (default or environment-specific)
     - secrets
   - Report missing variables for each environment.
   - For each optional variable, report its value if present, or “—” if not set.

4. **Output**
   - Generate two tables:
     - **Table 1:** Mandatory variables
       - Rows: mandatory variables
       - Columns: environments (dev, int, stg, prod)
       - Cells: found value, (secret), found value (default) if the default value is used, or “—” if missing
     - **Table 2:** Optional variables
       - Rows: optional variables
       - Columns: environments (dev, int, stg, prod)
       - Cells: found value, (secret), found value (default) if the default value is used, or “—” if not set
   - List of missing **mandatory variables**


## Example tables

**Mandatory variables:**

| Variable      | dev      | int      | stg              | prod     |
| ------------- | -------- | -------- | ---------------- | -------- |
| VAR1          | value    | value    | value (default)  | value    |
| VAR2          | (secret) | (secret) | (secret)         | (secret) |
| VAR3          | —        | —        | —                | —        |

**Optional variables:**

| Variable      | dev      | int      | stg      | prod     |
| ------------- | -------- | -------- | -------- | -------- |
| OPT_VAR1      | value    | —        | value    | —        |
| OPT_VAR2      | —        | —        | —        | —        |

## Notes
- Missing mandatory variables must be added to avoid runtime errors.
- Variables defined as secret must still be documented in the tables.
- Don't add any additional suggestions or proposed future actions. Limit yourself to the requested output.