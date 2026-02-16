---
name: create-agents-md
description: Generate hierarchical AGENTS.md structures for codebases. Use when user asks to create AGENTS.md files, analyze codebase for AI agent documentation, set up AI-friendly project documentation, or generate context files for AI coding assistants. Triggers on "create AGENTS.md", "generate agents", "analyze codebase for AI", "AI documentation setup", "hierarchical agents".
---

# AGENTS.md Generator

Generate hierarchical AGENTS.md structures optimized for AI coding agents with minimal token usage.

## Core Principles

1. **Root AGENTS.md is LIGHTWEIGHT** - Only universal guidance, links to sub-files (~100-200 lines max)
2. **Nearest-wins hierarchy** - Agents read closest AGENTS.md to file being edited
3. **JIT indexing** - Provide paths/globs/commands, NOT full content
4. **Token efficiency** - Small, actionable guidance over encyclopedic docs
5. **Sub-folder files have MORE detail** - Specific patterns, examples, commands

## Workflow

### Phase 1: Repository Analysis

Analyze and report:
1. **Repository type**: Monorepo, multi-package, or simple?
2. **Tech stack**: Languages, frameworks, key tools
3. **Major directories** needing own AGENTS.md:
   - Apps (`apps/web`, `apps/api`, `apps/mobile`)
   - Services (`services/auth`, `services/transcribe`)
   - Plugin (`plugin/auth`)
   - Packages (`packages/ui`, `packages/shared`)
   - Workers (`workers/queue`, `workers/cron`)
4. **Build system**: pnpm/npm/yarn workspaces? Turborepo? Lerna?
5. **Testing setup**: Jest, Vitest, Playwright, pytest?
6. **Key patterns**: Organization, conventions, examples, anti-patterns

Present as structured map before generating files.

### Phase 2: Root AGENTS.md

Create lightweight root (~100-200 lines):

```markdown
# Project Name

## Project Snapshot
[3-5 lines: repo type, tech stack, note about sub-AGENTS.md files]

## Root Setup Commands
[5-10 lines: install, build all, typecheck all, test all]

## Universal Conventions
[5-10 lines: code style, commit format, branch strategy, PR requirements]

## Security & Secrets
[3-5 lines: never commit tokens, .env patterns, PII handling]

## JIT Index
### Package Structure and Architecture
- Follow the repository pattern eg. API: `apps/api/` -> [see apps/api/AGENTS.md](apps/api/AGENTS.md)

```

### Phase 3: Sub-Folder AGENTS.md

For each major package, create detailed AGENTS.md:

```markdown
# Package Name

## Package Identity
[2-3 lines: what it does, primary tech]

## Setup & Run
[5-10 lines: install, dev, build, test, lint commands]

## Patterns & Conventions
[10-20 lines - MOST IMPORTANT SECTION]
- File organization rules
- Naming conventions
- Examples with actual file paths

## Key Files
[5-10 lines: important files to understand package]
Example:
- Auth: `src/auth/provider.tsx`
- API client: `src/lib/api.ts`
- Types: `src/types/index.ts`

## Common Gotchas
[3-5 lines if applicable]
- "Auth requires NEXT_PUBLIC_ prefix for client-side"
- "Always use @/ imports for absolute paths"

```

### Phase 4: Special Templates

#### Design System / UI Package
```markdown
## Design System
- Components: eg: `packages/ui/src/components/**`
- Use design tokens from eg: `packages/ui/src/tokens.ts`
- Component gallery: eg: `npm --filter @repo/ui storybook`
```

#### Database / Data Layer
```markdown
## Database
- ORM: Prisma / Drizzle / TypeORM
- Schema: `prisma/schema.prisma`
- Migrations: `pnpm db:migrate`
- Connection: via `src/lib/db.ts` singleton
- NEVER run migrations in tests
```

#### API / Backend Service
```markdown
## API Patterns
- REST routes: `src/routes/**/*.ts`
- Auth middleware: `src/middleware/auth.ts`
- Plugin: `src/plugin/auth.ts`
- Validation: Zod/JSON Schema/Typebox schemas in `src/schemas/**`
- Errors: `ApiError` from `src/errors.ts`
- Example: `src/routes/users/get.ts`
```

#### Testing
```markdown
## Testing
- Unit: `*.test.ts` colocated
- Integration: `tests/integration/**`
- E2E: `tests/e2e/**` (Playwright)
- Single test: `pnpm test -- path/to/file.test.ts`
- Mocks: `src/test/mocks/**` or `src/test/mock/**`
- Fixture: `src/test/fixtures/**`
```

## Output Format

Provide files in order:
1. Analysis Summary
2. Root AGENTS.md (complete)
3. Each Sub-Folder AGENTS.md (with file path)

Format:
```
---
File: `AGENTS.md` (root)
---
[content]

---
File: `apps/web/AGENTS.md`
---
[content]
```

## Quality Checklist

Before generating, verify:
- [ ] Root AGENTS.md under 200 lines
- [ ] Root links to all sub-AGENTS.md files
- [ ] Each sub-file has concrete examples (actual paths)
- [ ] Commands are copy-paste ready
- [ ] No duplication between root and sub-files
- [ ] JIT hints use actual patterns (ripgrep, find, glob)
- [ ] Every "DO" has real file example
- [ ] Every "DON'T" references real anti-pattern
- [ ] Pre-PR checks are single commands



# Best Practices

- Keep AGENTS.md files small and focused
- Use for project-specific conventions
- Prefer **short, concrete references** over long prose:
  - Link to project docs, specs, and runbooks
  - Point to example files or directories (e.g., `see src/api/users.ts for canonical pattern`)
  - Include the most important commands with exact CLI invocations
- Reference existing code examples when possible
- Update as project evolves

## Size Guidelines

- Keep AGENTS.md files under 200 lines when possible
- Focus on essential project-specific instructions
- Move detailed documentation to skills or project docs

## What to Avoid

- General programming advice (use skills instead)
- Framework documentation (reference official docs)
- Long examples (reference code files)
- Duplicate information from skills
- Anti-Patterns, omit these
  - "Welcome to..." or "This document explains..."
  - "You should..." or "Remember to..."
  - Obvious instructions ("run tests", "write clean code")
  - Explanations of why (just say what)
  - Long prose paragraphs

## Organization

### Project Root AGENTS.md

- Single source of truth for **global project conventions**
- Overall architecture and key domain concepts
- Primary commands: build, test, lint, typecheck, format
- Tech stack details (framework versions, package managers)
- Safety boundaries (what the agent must not touch: secrets, vendor dirs, prod configs)

### Folder-Scoped AGENTS.md

- Only add when a directory/package has **meaningfully different rules** than the root
- Directory-specific rules (e.g., `apps/web/`, `packages/api/`, `infra/`)
- Package-specific instructions (in monorepos)
- Test-specific guidance (e.g., how to run slow/integration tests locally)
- Avoid duplicating root content; reference root conventions and add only the deltas


