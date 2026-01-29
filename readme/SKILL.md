---
name: readme
description: Create or update README.md by analyzing the current state of the repository. Keep documentation accurate, concise, and aligned with the codebase.
---

# README Management Skill

## Purpose
This skill guides the agent in **creating or updating the README.md file** so that it accurately reflects the **current version of the codebase**.

The goal is to produce clear, reliable, and developer-oriented documentation without inventing features or behavior.

---

## When to Use
Use this skill when:
- The user asks to create a README.md
- The user asks to update or fix an existing README.md
- Documentation is missing, outdated, or inconsistent with the codebase

---

## Primary Responsibility
Ensure that `README.md`:
- Matches the current functionality of the repository
- Describes only verifiable behavior
- Is useful for onboarding new contributors or users

---

## Scope of Analysis
Analyze the repository as a whole, including:
- Source code and entry points
- Configuration files and environment variables
- Dependency manifests and lock files
- Build, run, and test scripts
- CI/CD configuration (if present)
- Existing documentation files

---

## Update Strategy

1. **Read Existing README.md (if present)**
   - Preserve accurate sections
   - Update or remove outdated information

2. **Align With the Codebase**
   - Installation steps must match real dependencies
   - Usage instructions must match real commands or APIs
   - Configuration must reflect actual config files

3. **Be Conservative**
   - Do not guess undocumented behavior
   - Omit unclear sections rather than speculating

4. **Prefer Clarity Over Completeness**
   - Document what is important and verifiable
   - Avoid unnecessary verbosity

---

## Recommended README Structure
Include only sections relevant to the project:

- Project Title
- Description
- Features (only if clearly identifiable)
- Requirements / Prerequisites
- Installation
- Usage
- Project Structure (use folder tree, show only folder do no include single file)
- Configuration
- Testing
- Build / Deployment (if applicable)

---

## Accuracy Rules
- Do not invent features, commands, or flags
- Do not document commented-out or unused code
- Do not describe future plans or TODOs
- If assumptions are necessary, clearly label them

---

## Tool Usage Guidance
When available, use repository inspection tools to:
- Browse files and directories
- Read source and configuration files
- Search for scripts, entry points, and commands

---

## Output Rules
- Modify **only** `README.md`, unless explicitly instructed otherwise
- Output valid Markdown
- Ensure the README reflects the **current repository state**

---

## Consistency Rule
If this skill is applied multiple times, the resulting README should remain:
- Structurally consistent
- Stylistically uniform
- Free of duplicated or conflicting information
