---
name: pr-review
description: Deterministic structured code review comparing the current branch against a base branch (default main). Identifies bugs, regressions, fragile patterns, missing tests, and optionally applies fixes with corresponding tests. Triggers on "review changes", "check for bugs", "code review", "review this branch", "review before PR".
---

# PR Review Skill

Performs a structured, reproducible code review comparing the current branch against a base branch (default: `main`).

The review focuses on:
- Correctness and regressions
- Security implications
- Contract changes (explicit or implicit)
- Fragility and coupling
- Test coverage gaps
- Production risk

This skill does **not** modify code unless explicitly instructed after presenting findings.

------------------------------------------------------------------------

# Execution Contract

## Preconditions

-   Must be inside a Git repository.
-   Working tree must not contain unrelated staged changes.
-   Base branch must exist locally (`main` by default).

If any condition fails: - Stop and explain the blocking issue. - Do not guess or proceed partially.

------------------------------------------------------------------------

# Deterministic Workflow

## Step 1 --- Resolve Branch Context

```bash
git branch --show-current
git rev-parse --verify main
git log main..HEAD --oneline
git diff main...HEAD --stat
```

If the user specifies a different base branch: - Replace `main` consistently. - Confirm it exists before proceeding.

------------------------------------------------------------------------

## Step 2 --- Compute Review Scope

```bash
git diff main...HEAD
```

For large diffs: - Segment per file. - Prioritize:
1. Core domain logic
2. Persistence layer
3. Auth/security logic
4. Public API surface
5. Tests

Ignore: - Pure formatting-only diffs - Lockfile changes (unless dependency upgrade risk)

------------------------------------------------------------------------

## Step 3 --- Code Analysis Framework

For each modified file:

### A. Behavioral Changes

-   Has public contract changed?
-   Are return types or thrown errors different?
-   Are edge cases handled differently?
-   Any implicit breaking change?

### B. Runtime Safety

-   Unhandled exceptions
-   Null/undefined regressions
-   Async race conditions
-   Shared mutable state

### C. Security Review

-   Authorization bypass
-   Missing validation
-   Injection risks
-   Skipped encryption/mapping layers
-   Sensitive data logged

### D. Data & Persistence Risks

-   Schema changes
-   Migration compatibility
-   Backward compatibility
-   Silent data corruption risks

### E. Fragility & Coupling

-   Hidden assumptions
-   Cross-module implicit dependencies
-   Feature-flag dependent required config
-   Non-idempotent logic

### F. Observability

-   Misleading log levels
-   Missing error context
-   Silent failures

------------------------------------------------------------------------

## Step 4 --- Test Coverage Analysis

For each changed module:

```bash
find test/ -name "*.test.*"
```

Evaluate:

-   Are new branches tested?
-   Are edge cases tested?
-   Do modified contracts update existing tests?
-   Does test suite still reflect intended behavior?
-   Are regression tests required?

Every bug fix must require: - A failing test first - Then the fix - Then test passing

------------------------------------------------------------------------

# Severity Classification

  Level      Emoji   Definition
  ---------- ------- ----------------------------------------------------
  Critical   🔴      Production crash, data loss, security breach
  High       🟠      Breaking change, incorrect behavior in common path
  Medium     🟡      Fragile logic, hidden coupling
  Low        ℹ️      Style, readability, minor optimization

------------------------------------------------------------------------

# Required Output Format (Strict)

```markdown
## Code Review — <current-branch> vs main

Commit Range:
- <commit list>

Files Reviewed:
- file1.ts
- file2.ts

---

### 🔴 Critical #1 — <Short Title>

File: path/to/file.ts:LNN

```ts
// relevant snippet
```

Problem:
Clear explanation of what is wrong.

Impact:
Concrete scenario affected (who breaks and when).

Root Cause:
Why this was introduced.

Fix:
Specific code-level correction.

Test Required:
Name of test + scenario to validate.

---

### 🟠 High #2 — <Short Title>
...
```

------------------------------------------------------------------------

# Missing Tests Section (Mandatory)

```markdown
## Missing Tests

1. <Behavior>
   - Test Name: should_<behavior>_when_<condition>
   - File: test/module.test.ts
   - Purpose: regression protection

2. ...
```

------------------------------------------------------------------------

# Priority Summary (Mandatory)

```markdown
## Priority Summary

| # | Severity | File | Description | Must Fix Before Merge |
|---|----------|------|-------------|-----------------------|
| 1 | 🔴 | file.ts | Null dereference in X | Yes |
| 2 | 🟠 | api.ts | Breaking response contract | Yes |
| 3 | 🟡 | util.ts | Hidden coupling | No |
```

------------------------------------------------------------------------

# Fix Application Protocol

After presenting findings:

1.  Ask user:

    -   Apply all critical fixes?
    -   Apply all high severity fixes?
    -   Apply everything?

2.  Apply fixes incrementally.

3.  Add/update tests for each fix.

4.  Run targeted tests:

    ```bash
    npx vitest run test/path/to/test.ts
    ```

5.  Confirm no regression.

Never: - Batch unrelated fixes without confirmation. - Modify tests to make them pass unless behavior change is intentional and explained.

------------------------------------------------------------------------

# Anti-Patterns (Always Flag)

-   Throwing on not-found when contract expects nullable
-   Writes bypassing transformation layer
-   Required config gated behind disabled feature flag
-   Silent catch blocks
-   Logging secrets
-   Mutating input parameters
-   Returning partially initialized objects
-   Stale tests asserting outdated behavior

------------------------------------------------------------------------

# Guardrails

-   Do not assume intent --- verify from diff.
-   Do not invent missing code.
-   If context is insufficient, explicitly state what is unknown.
-   Never apply speculative refactors.
-   Avoid stylistic nitpicks unless they introduce risk.

------------------------------------------------------------------------

# Tone & Communication Rules

-   Direct and technical.
-   No vague language unless uncertainty is real.
-   Every finding must include:
    -   Impact
    -   Root cause
    -   Concrete fix
-   Clearly distinguish:
    -   Must fix before merge
    -   Recommended improvements

------------------------------------------------------------------------

# Optional Advanced Mode

If the user requests deeper analysis:

-   Architectural risk analysis
-   Coupling graph analysis
-   Change risk scoring
-   Merge safety assessment
-   Production readiness checklist
