---
name: init
description: "Initialize the .wingman project directory with configuration, templates, and git setup."
argument-hint: "Optional: preferences like 'timestamp numbering' or 'no git'"
metadata:
  author: "github-spec-kit"
  source: "speckit-init"
user-invocable: true
disable-model-invocation: true
---


## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

Initialize a new project for use with Wingman by scaffolding the `.wingman/` directory, collecting user preferences, and optionally initializing a Git repository.

This skill is **idempotent** — running it again on an already-initialized project will skip existing files and only create missing ones. It never overwrites existing configuration.

## Execution Flow

### 1. Check for existing initialization

Check if `.wingman/` already exists at the project root:
- If it exists and contains `init-options.json`, inform the user that the project is already initialized and list what's present
- Offer to fill in any missing pieces (e.g., templates directory, git config override) without overwriting existing files
- If the user passed arguments requesting specific changes (e.g., "switch to timestamp numbering"), apply those changes to the existing configuration

### 2. Collect preferences

Ask the user for their preferences. If the user provided them via `$ARGUMENTS`, use those directly. Otherwise, present these questions:

**Q1: Branch numbering strategy**

| Option | Value | Description |
|--------|-------|-------------|
| A      | `sequential` | Branches and spec directories use sequential numbers: `001-`, `002-`, ... (default) |
| B      | `timestamp`  | Use timestamps: `20260410-143022-`, ... |

**Q2: Auto-commit behavior**

| Option | Value | Description |
|--------|-------|-------------|
| A      | `prompt` | Ask before each auto-commit (default) |
| B      | `silent` | Commit automatically without asking |
| C      | `disabled` | Never auto-commit |

**Q3: Initialize Git repository?**

| Option | Value | Description |
|--------|-------|-------------|
| A      | `yes` | Initialize git repo if one doesn't exist (default) |
| B      | `no`  | Skip git initialization |

**Q4: Script type**

| Option | Value | Description |
|--------|-------|-------------|
| A      | `sh`  | POSIX Shell / Bash (default on macOS and Linux) |
| B      | `ps`  | PowerShell (default on Windows) |

Default: `sh` on macOS/Linux, `ps` on Windows. Detect the current platform and use the appropriate default.

If the user says something like "just use defaults" or provides no arguments, use all defaults without asking.

### 3. Create the `.wingman/` directory structure

Create the following structure at the project root, skipping any files/directories that already exist:

```
.wingman/
├── init-options.json
├── templates/
│   ├── spec-template.md
│   ├── plan-template.md
│   ├── tasks-template.md
│   ├── checklist-template.md
│   ├── constitution-template.md
│   ├── agent-file-template.md
│   └── overrides/
└── extensions/
    └── git/
        └── git-config.yml
```

**File contents:**

#### `.wingman/init-options.json`

Copy from `${CLAUDE_PLUGIN_ROOT}/assets/init-options.json` and apply user preferences from step 2:

```json
{
  "ai": "claude",
  "ai_skills": true,
  "branch_numbering": "<sequential|timestamp>",
  "integration": "claude",
  "script": "<sh|ps>"
}
```

Update `branch_numbering` and `script` to match the user's choices from Q1 and Q4.

#### `.wingman/extensions/git/git-config.yml`

Copy from `${CLAUDE_PLUGIN_ROOT}/assets/git-config.yml` and apply user preferences:

- If auto-commit preference is `prompt`: set all `optional: true` (this is the default)
- If auto-commit preference is `silent`: set all `optional: false`, `enabled: true`
- If auto-commit preference is `disabled`: set `default: false` and all `enabled: false`

#### `.wingman/templates/`

Copy all 6 template files from `${CLAUDE_PLUGIN_ROOT}/assets/templates/` to `.wingman/templates/`:
- `spec-template.md`
- `plan-template.md`
- `tasks-template.md`
- `checklist-template.md`
- `constitution-template.md`
- `agent-file-template.md`

These are the project-local copies that `resolve_template()` uses as priority 4 (core). Users can discover and edit them directly.

#### `.wingman/templates/overrides/`

Create as an empty directory. This is where users place template overrides that take highest priority in the template resolution stack (priority 1).

### 4. Create the `specs/` directory

Create `specs/` at the project root if it doesn't exist. This is where feature specifications will be stored.

### 5. Git initialization

If the user chose to initialize Git (or accepted the default):
- Invoke `wingman:git-initialize` to set up the repository
- This is a no-op if a git repo already exists

### 6. Report completion

Report to the user:

```
✓ Wingman initialized

  .wingman/init-options.json      — branch numbering: <choice>
  .wingman/extensions/git/        — auto-commit: <choice>
  .wingman/templates/overrides/   — ready for custom templates
  specs/                          — feature specs directory
  git                             — <initialized | already exists | skipped>

Next steps:
  • wingman:constitution — define project principles (optional)
  • wingman:specify <description> — create your first feature spec
```

## Idempotency Rules

- **Never overwrite** existing files — only create missing ones
- If `.wingman/init-options.json` exists, read it to determine current settings and only apply explicitly requested changes
- If `.wingman/extensions/git/git-config.yml` exists, leave it as-is unless the user explicitly requested auto-commit changes
- If `specs/` exists, skip creation
- If git repo exists, skip initialization
- Report what was created vs what was skipped

## Post-Execution

Invoke `wingman:git-commit` after completion with event name `after_init`.
