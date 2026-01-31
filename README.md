# AI Skills

Folder containing my personal skills (scripts, helpers and configurations) used by my agents.

## Available Skills

| Skill Name | Description |
|------------|-------------|
| **create-agents-md** | Generate hierarchical AGENTS.md structures for codebases. Creates organized, token-efficient documentation with root files and subfolder-specific instructions following JIT indexing and nearest-wins hierarchy principles. |
| **agent-md-refactor** | Refactor bloated AGENTS.md, CLAUDE.md, or similar agent instruction files using progressive disclosure principles. Splits monolithic files into organized, linked documentation with contradiction detection and deletion recommendations. |
| **envVars** | Validate the coverage of required and optional environment variables between code and Helm/K8s configurations. Generates comparison tables for mandatory and optional variables across different environments (dev, int, stg, prod). |
| **readme** | Create or update README.md by analyzing the current state of the repository. Ensures documentation is accurate, concise, and aligned with the actual codebase without inventing features. |

## Structure
- Each skill is in a subfolder with its own code and minimal documentation.

## How to Use These Skills

### Installation Locations

You can place skills in different locations depending on your needs:

- **Global skills (user-level):** Place in `~/.copilot/skills/` for skills shared across all your projects
- **Project-specific skills:** Place in `.github/skills/` within your repository for project-specific skills
- **Legacy compatibility:** `~/.claude/skills/` is also supported but `~/.copilot/skills/` is preferred

### Installation Steps

1. **Choose a location** based on your needs (global or project-specific)
2. **Copy the skill folder** from this repository to your chosen location
   ```bash
   # Example: Install the readme skill globally
   cp -r readme ~/.copilot/skills/readme
   
   # Example: Install the create-agents-md skill in a project
   cp -r create-agents-md /path/to/your/project/.github/skills/create-agents-md
   ```
3. **Reload your IDE** or restart the GitHub Copilot CLI for the new skills to be detected
4. GitHub Copilot will automatically load and use relevant skills based on your task context

### Skill Structure

Each skill directory contains:
- `SKILL.md` - Main skill definition with YAML frontmatter and instructions
- Additional supporting files or scripts specific to that skill

For more information, see the [GitHub Copilot Agent Skills documentation](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills).

