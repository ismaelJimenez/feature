<p align="center">
  <img src="images/WingMan_Icon.png" alt="Wingman Icon" width="200" />
</p>

# Wingman

A comprehensive feature development toolkit providing structured workflows for specification, planning, implementation, and quality assurance — from brainstorm to production code.

## What it does

Wingman guides you through the full feature development lifecycle:

0. **Init** — Initialize project directory with configuration and preferences
1. **Brainstorm** — Explore ideas with structured brainstorming sessions
2. **Specify** — Create feature specifications from natural language descriptions
3. **Clarify** — Identify and resolve underspecified areas via targeted questions
4. **Plan** — Generate implementation plans with architecture and file structure
5. **Tasks** — Break plans into actionable, dependency-ordered task lists
6. **Checklist** — Generate quality checklists tailored to the feature
7. **Analyze** — Cross-artifact consistency and quality analysis
8. **Implement** — Execute tasks phase-by-phase with automatic commits
9. **Tasks to Issues** — Convert tasks to GitHub issues

Additional project governance and Git automation skills run automatically as part of the workflow.

## Installation

Install with [Summon](https://github.com/ai-summon/summon):

```
summon install wingman
```

## Usage

Start by initializing your project, then invoke skills by name:

```
/wingman:init
/wingman:specify Build a REST API for user management
/wingman:plan
/wingman:tasks
/wingman:implement
```

### Recommended Workflow Order

0. `wingman:init` — Initialize project directory and preferences (one-time)
1. `wingman:constitution` — Define project principles (optional, one-time)
2. `wingman:specify` — Create feature specification
3. `wingman:clarify` — Resolve ambiguities in the spec
4. `wingman:plan` — Generate implementation plan
5. `wingman:tasks` — Break plan into ordered tasks
6. `wingman:checklist` — Generate quality checklist
7. `wingman:analyze` — Cross-artifact consistency check
8. `wingman:implement` — Execute all tasks
9. `wingman:tasks-to-issues` — Convert tasks to GitHub issues

During development, run `/reload-plugins` after edits to pick up changes.

## Components

### Project Setup Skills

| Skill | Description |
|-------|-------------|
| `init` | Initialize `.wingman/` project directory with configuration, templates, and git setup |
| `constitution` | Define project principles and governance |

### Workflow Skills

| Skill | Description |
|-------|-------------|
| `specify` | Create feature specifications from natural language |
| `clarify` | Identify and resolve underspecified areas |
| `plan` | Generate implementation plans with architecture |
| `tasks` | Break plans into dependency-ordered task lists |
| `implement` | Execute tasks phase-by-phase |
| `checklist` | Generate quality checklists |
| `analyze` | Cross-artifact consistency analysis |
| `tasks-to-issues` | Convert tasks to GitHub issues |

### Git Skills (auto-invoked)

| Skill | Description |
|-------|-------------|
| `git-initialize` | Initialize git repository |
| `git-feature` | Create feature branches |
| `git-commit` | Auto-commit changes |
| `git-validate` | Validate branch naming conventions |

### Standalone Skills

| Skill | Description |
|-------|-------------|
| `brainstorm` | Structured brainstorming and document generation |

## Output

Skills produce artifacts in a feature-specific `specs/NNN-feature-name/` directory:

- **`spec.md`** — Feature specification
- **`plan.md`** — Implementation plan with architecture and file structure
- **`tasks.md`** — Dependency-ordered task breakdown
- **`data-model.md`** — Entity definitions and relationships
- **`research.md`** — Technical decisions and constraints
- **`checklists/`** — Quality checklists
- **`contracts/`** — API specifications

The `brainstorm` skill produces documents in a `brainstorm/` directory:

- **`NN-topic-slug.md`** — Individual brainstorm documents
- **`00-overview.md`** — Auto-generated index of all sessions

## License

MIT

## Troubleshooting

### Setting up the plugin in VS Code Copilot Chat

Agent plugins in VS Code are currently in **preview**. For more information, see [Agent plugins in VS Code](https://code.visualstudio.com/docs/copilot/customization/agent-plugins) and [Agent Skills in VS Code](https://code.visualstudio.com/docs/copilot/customization/agent-skills).

1. Enable the feature: **Settings** → search `chat.plugins.enabled` → check the box
2. Add the marketplace to your `settings.json`:
   ```json
   "chat.plugins.marketplaces": [
       "ismaelJimenez/ai-dev-utils"
   ]
   ```
3. Open the Extensions view (`⇧⌘X` / `Ctrl+Shift+X`), search `@agentPlugins`, find `feature`, and install it
4. Type `/` in Copilot Chat — you should see `brainstorm` in the list

### VS Code Copilot Chat shows `/brainstorm` instead of `/feature:brainstorm`

This is expected. The `plugin:skill` namespacing (e.g., `/feature:brainstorm`) is a convention used by the Claude Code and Copilot CLI plugin systems, where it works correctly. In VS Code Copilot Chat, slash commands use the skill name directly (e.g., `/brainstorm`) — the plugin name prefix is not added automatically.
