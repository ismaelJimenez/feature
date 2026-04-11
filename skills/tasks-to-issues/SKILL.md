---
name: tasks-to-issues
description: "Convert existing tasks into actionable, dependency-ordered GitHub issues for the feature based on available design artifacts."
argument-hint: "Optional filter or label for GitHub issues"
metadata:
  author: "github-spec-kit"
  source: "speckit-taskstoissues"
---


## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Pre-Execution

Invoke `wingman:git-commit` with event `before_taskstoissues` before proceeding.

## Outline

1. Run `${CLAUDE_PLUGIN_ROOT}/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` from repo root and parse FEATURE_DIR and AVAILABLE_DOCS list. All paths must be absolute. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

   **IMPORTANT**: Before running scripts, set the environment variable:
   - **Bash**: `export WINGMAN_ROOT="${CLAUDE_PLUGIN_ROOT}"`
   - **PowerShell**: `$env:WINGMAN_ROOT = "${CLAUDE_PLUGIN_ROOT}"`

2. From the executed script, extract the path to **tasks**.

3. Get the Git remote by running:

```bash
git config --get remote.origin.url
```

> [!CAUTION]
> ONLY PROCEED TO NEXT STEPS IF THE REMOTE IS A GITHUB URL

4. For each task in the list, use the GitHub MCP server to create a new issue in the repository that is representative of the Git remote.

> [!CAUTION]
> UNDER NO CIRCUMSTANCES EVER CREATE ISSUES IN REPOSITORIES THAT DO NOT MATCH THE REMOTE URL

## Post-Execution

Invoke `wingman:git-commit` after completion with event name `after_taskstoissues`.
