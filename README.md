# AI Skills

A collection of the AI "skills" I use on my workspace

## Available skills

- **agent-md-refactor** — Refactor bloated AGENTS.md / CLAUDE.md files into a progressive-disclosure hierarchy (splits monolithic instruction files into organized, linked documents).
- **create-agents-md** — Generate hierarchical AGENTS.md structures optimized for AI agents (analyzes repo layout and creates root + per-package AGENTS.md files).
- **create-ux** — Facilitate UX design: workshops, decision workshops, spec templates, and standalone HTML/CSS prototypes.
- **env-vars-consistency-check** — Validate environment-variable schemas against Helm/CI/Kubernetes configs and report resolved values per environment.
- **fastify-best-practices** — Comprehensive Fastify rules and examples (plugins, routes, schemas, testing, performance, deployment) -> from [Matteo Collina Skills](https://github.com/mcollina/skills)
- **generate-postman-collection** — Analyze a codebase and produce a Postman Collection v2.1 JSON with realistic examples and grouped endpoints.
- **my-skill-creator** — Scaffolding helper to create new agent skills with proper frontmatter, templates, and recommended structure. -> from [Minko Gechev Skills Best Practice](https://github.com/mgechev/skills-best-practices)
- **skill-validator** — Audits an existing skill directory against the agentskills.io spec, checking metadata, file structure, instruction quality, and progressive disclosure. Generates a structured report with pass/fail findings and remediation steps.
- **humanizer** — Remove signs of AI-generated writing and rewrite text to sound more natural and human -> from [humanizer](https://github.com/blader/humanizer)
- **pr-description** — Generate clear PR titles and structured descriptions from diffs and branch context (note: folder name `pr-descripton` contains this skill; see on-disk spelling).
- **pr-review** — Deterministic, structured code review helper that identifies bugs, missing tests, and security or regression risks.
- **tdd** — Test-driven development guidance (red → green → refactor) and test-writing checklists -> from [Matt Pocock Skills](https://github.com/mattpocock/skills)
- **typescript-magician** — Advanced TypeScript expertise: generics, conditional types, type inference, and eliminating `any` -> from [Matteo Collina Skills](https://github.com/mcollina/skills)
- **update-readme** — Create or update a repository `README.md` by inspecting the codebase and aligning documentation with real commands and files.

Each skill lives in a subfolder and includes a `SKILL.md` that documents its behavior and configuration. Use the `description` field in each `SKILL.md` as the primary summary when choosing a skill.

## Structure

- `SKILL.md` — Primary skill definition and usage instructions.
- Other files — scripts, rules, templates or support code for the skill.

## Installation

Choose one of the installation locations:

- Global (user-level): `~/.copilot/skills/`
- Project-level: `.github/skills/` inside a repository

Examples:

```bash
# Install a skill globally
cp -r create-agents-md ~/.copilot/skills/create-agents-md

# Install project-scoped skills
cp -r generate-postman-collection /path/to/repo/.github/skills/generate-postman-collection
```

Reload your IDE or agent runtime so it rescans the skills directory.

## Notes & References
- [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Visual Studio Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [humanizer](https://github.com/blader/humanizer)
- [Matt Pocock Skills](https://github.com/mattpocock/skills)
- [Matteo Collina Skills](https://github.com/mcollina/skills)
- [Minko Gechev Skills Best Practice](https://github.com/mgechev/skills-best-practices)

