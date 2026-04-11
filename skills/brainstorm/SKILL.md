---
name: brainstorm
description: "You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent before especification."
---

# Brainstorming Ideas Into Clarity

Turn rough ideas into well-structured brainstorm documents through collaborative dialogue. The brainstorm document is the deliverable — it captures problem framing, approaches considered, decisions made, and open threads. From there, the user takes it to any downstream tool or workflow.

## Pre-Execution

Invoke `wingman:git-commit` with event `before_brainstorm` before proceeding.

## Checklist

Follow these steps in order. Do not skip steps.

1. **Explore project context** — read README, project docs, and files directly related to the topic; check recent commits only if the topic involves recently changed code
2. **Check for related brainstorms** — scan `brainstorm/` for existing docs on similar topics, offer to update or create new
3. **Assess complexity** — quick decision (2–3 options, clear trade-offs) or complex exploration (multiple subsystems, unclear constraints); scale the remaining steps accordingly
4. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
5. **Propose 2–3 approaches** — with trade-offs and a recommendation
6. **Present findings & recommendations** — scaled to complexity, get user approval incrementally
7. **User approves brainstorm summary** — confirm the session capture before writing
8. **Write brainstorm document** — persist session summary to `brainstorm/NN-topic-slug.md`
9. **Update overview** — create or refresh `brainstorm/00-overview.md` with index, open threads, parked ideas (skip for quick decisions if no overview exists yet)
10. **Suggest next steps** — the brainstorm document can be taken to any downstream tool or workflow

## The Process

### Understanding the idea

- Review README, project docs, and files directly related to the topic. Check recent commits only if the topic involves recently changed code. Limit context gathering to a few targeted reads.
- Assess scope first: if the request covers multiple independent subsystems, flag it and help decompose into sub-topics before deep-diving. Each sub-topic goes through the normal flow.
- One question per message. Prefer multiple choice. Focus on purpose, constraints, success criteria, edge cases, and dependencies.

### Assessing complexity

After initial context and questions, determine the scale:

- **Quick decision** — 2–3 clear options, straightforward trade-offs: propose approaches immediately, write a lightweight document, skip overview update if no overview exists yet.
- **Complex exploration** — multiple subsystems, unclear constraints, significant design decisions: follow the full process with incremental validation at each stage.

### Exploring approaches

- Propose 2–3 approaches with trade-offs, leading with the recommendation
- Explore: core requirements vs. nice-to-have, error cases, integration points, success criteria

### Presenting findings & recommendations

- Scale each section to its complexity: a few sentences if straightforward, up to 200-300 words if nuanced
- Ask after each section whether it looks right so far
- Cover: problem framing, approaches considered, recommended approach, trade-offs, open questions

## Brainstorm Document Structure

```markdown
# Brainstorm: [Topic]

**Date:** YYYY-MM-DD
**Status:** active | parked | abandoned | completed

## Problem Framing
[What problem is being explored and why it matters]

## Approaches Considered

### A: [Approach Name]
- Pros: ...
- Cons: ...

### B: [Approach Name]
- Pros: ...
- Cons: ...

## Decision
[What was chosen and why, or "Parked: [reason]" if no decision was reached]

## Open Threads
- [Unresolved question or idea that needs further exploration]
```

**Status values:** `active` (being pursued), `parked` (may revisit), `abandoned` (not pursuing), `completed` (clear decision reached)

## Overview Document Structure

`brainstorm/00-overview.md` provides a navigable index of all sessions:

```markdown
# Brainstorm Overview

Last updated: YYYY-MM-DD

## Sessions

| # | Date | Topic | Status |
|---|------|-------|--------|
| 01 | YYYY-MM-DD | topic-slug | completed |

## Open Threads
- [Thread description] (from #NN)

## Parked Ideas
- [Idea description] (#NN)
  Reason: [why parked]
```

## Revisit Detection

During step 2, scan `brainstorm/` for existing `NN-*.md` files. Compare the current topic against existing slugs by keyword overlap. If a related document is found, ask the user: **create new** or **update existing**.

If updating, append a new dated section to the existing document instead of creating a new file:

```markdown
---

## Revisit: YYYY-MM-DD

### Updated Problem Framing
[How understanding has evolved]

### New Approaches Considered
...

### Updated Decision
...

### Open Threads
- [New or updated threads]
```

## When to Skip Brainstorming

Not every task needs a brainstorm session. Skip this skill when:

- The task is a bug fix with a clear root cause
- Explicit specs exist that leave no design decisions
- The change is purely mechanical (rename, move, reformat)

If the task does not involve design decisions or trade-offs, skip this skill.

## Writing the Brainstorm Document

Write the brainstorm document at session end — this step is NOT optional.

- **Naming:** `brainstorm/NN-topic-slug.md` — next sequential number (no gap-filling), slug is lowercase hyphenated 2–4 words
- **Status:** determined by session outcome (see status values above)
- **Format:** use the Brainstorm Document Structure above
- **Persist:** write the document to disk

## Updating the Overview

Update the overview after every brainstorm write or update — this step is NOT optional for complex explorations. For quick decisions, skip if no overview exists yet.

Regenerate `brainstorm/00-overview.md` by scanning all `NN-*.md` files — extract date, status, open threads, and parked ideas. Build using the Overview Document Structure above.

## Post-Execution

Invoke `wingman:git-commit` after completion with event name `after_brainstorm`.

## Incomplete Session Handling

If the session had no meaningful interaction (no approaches explored, no clarifying questions answered), do NOT prompt to save.

For sessions with meaningful interaction, ask: **"Save this brainstorm session?"**
- **Save as parked** — write document with status `parked`, update overview
- **Save as abandoned** — write document with status `abandoned`, update overview
- **Discard** — no artifacts created

## Key Principles

- **One question at a time** — never overwhelm with multiple questions
- **Multiple choice preferred** — easier to answer than open-ended when possible
- **YAGNI ruthlessly** — remove unnecessary features from consideration
- **Explore alternatives** — always propose 2–3 approaches before settling
- **Incremental validation** — present sections, get approval before moving on
- **Capture decisions** — record what was decided and why, not just the final choice
- **Scale to complexity** — lightweight process for quick decisions, full ceremony for complex explorations
- **Stay focused** — the brainstorm document is the deliverable; keep it sharp
