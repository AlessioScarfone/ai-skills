# AI Skills

A collection of small, focused "skills" (scripts, helpers, and configurations) designed to be loaded by agent tooling such as GitHub Copilot or similar local agent runners.

## Available skills

- **agent-md-refactor** — Refactors large AGENTS.md / CLAUDE.md files into smaller, progressive-disclosure docs.
- **create-agents-md** — Generates hierarchical AGENTS.md structures for repositories and teams.
- **create-skill** — Scaffolding utility to create a new skill with recommended files and metadata. => [Matt Pocock Skills](https://github.com/mattpocock/skills)
- **create-ux** — Facilitate UX design sessions with structured brainstorming, design decision workshops, specification writing, and standalone HTML/CSS prototyping.
- **env-vars-consistency-check** — Compares application env var schemas with Helm/Kubernetes configs across environments.
- **fastify** — Collection of Fastify-focused rules and helpers (security, plugins, testing, etc.) => from [Matteo Collina Skills](https://github.com/mcollina/skills)
- **humanizer** — Utilities to make AI-generated text sound more natural and human. => from [humanizer](https://github.com/blader/humanizer)
- **pr-descripton** — Templates and generators for standardized PR descriptions and checklists.
- **pr-review** — Deterministic code-review helpers to surface regressions and missing tests.
- **readme** — Creates or updates README.md by analyzing the repository and its files.
- **typescript-magician** — Advanced TypeScript type utilities and rules guidance. => from [Matteo Collina Skills](https://github.com/mcollina/skills)
- **tdd** — Test-driven development helpers (red-green-refactor loop). => from [Matt Pocock Skills](https://github.com/mattpocock/skills)

Each skill lives in a subfolder and includes a `SKILL.md` that documents its behavior and configuration.

## Structure

- `SKILL.md` — Primary skill definition and usage instructions.
- Other files — scripts, rules, or support code for the skill.

## Installation

Choose one of the installation locations:

- Global (user-level): `~/.copilot/skills/`
- Project-level: `.github/skills/` inside a repository

Examples:

```bash
# Install the readme skill globally
cp -r readme ~/.copilot/skills/readme

# Install project-scoped skills
cp -r create-agents-md /path/to/repo/.github/skills/create-agents-md
```

Reload your IDE or agent runtime so it rescans the skills directory.

## References

- [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Visual Studio Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [humanizer](https://github.com/blader/humanizer)
- [Matt Pocock Skills](https://github.com/mattpocock/skills)
- [Matteo Collina Skills](https://github.com/mcollina/skills)