---
name: git-commit
description: Auto-commit changes after a Wingman skill completes
metadata:
  author: github-spec-kit
  source: speckit-git-commit
user-invocable: true
disable-model-invocation: true
---

# Auto-Commit Changes

Automatically stage and commit all changes after a Wingman skill completes.

## Behavior

This skill is invoked after (or before) core commands. It:

1. Determines the event name from the calling context (e.g., `after_specify`, `before_plan`)
2. Checks `${CLAUDE_PLUGIN_ROOT}/assets/git-config.yml` for the `auto_commit` section
3. Looks up the specific event key to see if auto-commit is enabled
4. Falls back to `auto_commit.default` if no event-specific key exists
5. Uses the per-command `message` if configured, otherwise a default message
6. Depending on the `enabled` and `optional` fields:
   - `enabled: false` → skips (no commit, no prompt)
   - `enabled: true`, `optional: false` → stages and commits silently
   - `enabled: true`, `optional: true` → outputs a prompt signal for the agent to present to the user

## Execution

Determine the event name from the skill that invoked this, then run the script:

**IMPORTANT**: Before running scripts, set the environment variable:
- **Bash**: `export WINGMAN_ROOT="${CLAUDE_PLUGIN_ROOT}"`
- **PowerShell**: `$env:WINGMAN_ROOT = "${CLAUDE_PLUGIN_ROOT}"`

- **Bash**: `${CLAUDE_PLUGIN_ROOT}/scripts/bash/git/auto-commit.sh <event_name>`
- **PowerShell**: `${CLAUDE_PLUGIN_ROOT}/scripts/powershell/git/auto-commit.ps1 <event_name>`

Replace `<event_name>` with the actual event (e.g., `after_specify`, `before_plan`, `after_implement`).

## Configuration

In `${CLAUDE_PLUGIN_ROOT}/assets/git-config.yml`:

```yaml
auto_commit:
  default: false          # Global toggle — set true to enable for all commands
  after_specify:
    enabled: true          # Override per-command
    optional: false        # Silent commit (no user interaction)
    message: "[Wingman] Add specification"
  before_plan:
    enabled: true
    optional: true         # Prompt user before committing
    message: "[Wingman] Save progress before planning"
    prompt: "Save your work before planning?"
  after_plan:
    enabled: false
    message: "[Wingman] Add implementation plan"
```

## Graceful Degradation

- If Git is not available or the current directory is not a repository: skips with a warning
- If no config file exists: skips (disabled by default)
- If no changes to commit: skips with a message
