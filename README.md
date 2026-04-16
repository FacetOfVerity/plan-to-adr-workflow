# Plan to ADR Workflow

A Claude Code plugin for structured development planning with ADR (Architecture Decision Records) support.

## Overview

Two flows are supported side by side:

```text
┌──────────────┐   ┌───────────────┐   ┌──────────────────┐   ┌─────────┐
│  Plan Mode   │──▶│ /commit-plan  │──▶│ /implement-plan  │──▶│   ADR   │
│ (discussion) │   │    (save)     │   │    (execute)     │   │(archive)│
└──────────────┘   └───────────────┘   └──────────────────┘   └─────────┘

┌──────────────┐   ┌───────────────┐   ┌─────────┐
│     Chat     │──▶│  /commit-adr  │──▶│   ADR   │
│  (decision)  │   │    (record)   │   │ (direct)│
└──────────────┘   └───────────────┘   └─────────┘
```

## Installation

### Via FacetOfVerity Marketplace

```text
/plugin marketplace add FacetOfVerity/facetofverity-marketplace
/plugin install plan-to-adr-workflow@facetofverity-marketplace
```

Verify installation:

```text
/plugin list
```

## Skills

- `/commit-plan` — Save plan from current discussion.
- `/implement-plan` — Execute saved plan and archive as ADR on completion.
- `/commit-adr` — Record an ADR directly from chat (no plan step).

## When to Use

**Use `/commit-plan` + `/implement-plan` for:**

- New features affecting multiple files.
- Refactoring existing code.
- Architectural changes.
- Complex bug fixes.

**Use `/commit-adr` for:**

- Design decisions that won't be implemented right now.
- Decisions made and implemented in the same session without a plan.
- Retroactive documentation of past work.

**Not needed for:**

- Minor fixes (typos, formatting).
- Single-file changes.
- Quick fixes.

## Workflow: Plan → Implement → ADR

### Step 1: Plan Mode

Press `Shift+Tab` or type `/plan` to enter planning mode.

Claude will:

1. **Explore** — Study codebase, find relevant files.
2. **Clarify** — Ask questions about requirements.
3. **Design** — Propose implementation approach.
4. **Agree** — Discuss and refine the plan.

### Step 2: /commit-plan

After agreeing on a plan, save it:

```text
/commit-plan
```

Creates `docs/plans/active/YYYY-MM-DD_name.md` using [`skills/commit-plan/template.md`](skills/commit-plan/template.md):

- Context and Q&A from discussion.
- Tasks breakdown.
- Affected files.
- Risks and consequences.

### Step 3: /implement-plan

When ready to implement:

```text
/implement-plan
```

Claude will:

1. Show available plans.
2. Execute tasks sequentially.
3. Mark progress (`[ ]` → `[x]`).
4. Run tests after each phase.
5. Move completed plan to `docs/ADRs/YYYY-MM/`.

## Workflow: Chat → /commit-adr → ADR

Use when a plan-and-execute cycle isn't needed:

```text
/commit-adr
```

Claude will:

1. Analyze the current discussion.
2. Determine status (`Accepted` / `Proposed` / `Superseded`) and context type (`Design-only` / `Implemented` / `Retroactive`).
3. Fill in [`skills/commit-adr/template.md`](skills/commit-adr/template.md).
4. Save to `docs/ADRs/YYYY-MM/YYYY-MM-DD_name.md`.

## Folder Structure

The plugin creates and uses this structure in your project:

```text
docs/
├── plans/
│   ├── README.md        # Instructions
│   ├── STATUS.md        # Current status and roadmap
│   ├── active/          # Plans in progress
│   └── backlog/         # Deferred ideas
└── ADRs/
    └── YYYY-MM/         # ADRs (from /implement-plan archival or /commit-adr)
```

## Example: User Authentication Feature

```text
1. You: "Need to add OAuth2 authentication with Google and GitHub"

2. [Shift+Tab] — enter plan mode
   Claude explores code, asks questions:
   - Which OAuth providers?
   - How to store tokens?
   - What's the callback URL structure?
   → Agree on plan in chat

3. /commit-plan
   → docs/plans/active/2025-01-15_oauth-authentication.md

4. /implement-plan
   → Claude executes phases: DB schema, OAuth config, Routes, UI
   → Marks progress in plan file

5. Completion
   → Plan moves to docs/ADRs/2025-01/
   → Now it's an ADR with decision history
```

## Example: Direct ADR

```text
1. You: "We should standardize on ULIDs for all new primary keys. Existing UUID tables stay."

2. [short discussion of tradeoffs]

3. /commit-adr
   → docs/ADRs/2025-01/2025-01-15_ulid-for-new-primary-keys.md
   → Status: Accepted, Context Type: Design-only
```

## Templates

Templates live inside their owning skill folders and are referenced from SKILL.md via `${SKILL_ROOT}/template.md`. Edit them to change plan / ADR shape without touching skill logic.

| Template | Used by | Sections |
| --- | --- | --- |
| [`commit-plan/template.md`](skills/commit-plan/template.md) | `/commit-plan` | Metadata · Context · Solution · Tasks · Affected Files · Consequences · Testing · Documentation |
| [`commit-adr/template.md`](skills/commit-adr/template.md) | `/commit-adr` | Metadata · Context · Decision · Alternatives · Consequences · References |

## Language Support

- **Plan / ADR content**: Written in the user's language (auto-detected).
- **Filenames**: Always English (kebab-case).
- **Status / Context Type terms**: Kept in English.

## ADR — Architecture Decision Records

ADRs capture the *why* behind decisions:

- **Memory** — Why decisions were made.
- **Onboarding** — New devs understand history.
- **Revision** — Review decisions with new context.

Two paths lead to an ADR:

- `/implement-plan` — plan is archived as ADR upon completion.
- `/commit-adr` — ADR is written directly without a plan step.

## License

MIT
