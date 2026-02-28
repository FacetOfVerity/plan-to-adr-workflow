# Plan to ADR Workflow

A Claude Code plugin for structured development planning with ADR (Architecture Decision Records) support.

## Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Plan Mode     │───▶│  /commit-plan   │───▶│ /implement-plan │───▶│      ADR        │
│  (discussion)   │    │    (save)       │    │   (execute)     │    │   (archive)     │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Installation

### Via FacetOfVerity Marketplace (recommended)

```
/plugin marketplace add FacetOfVerity/skills-marketplace
/plugin install plan-to-adr-workflow@skills-marketplace
```

### Direct

```
/plugin marketplace add FacetOfVerity/plan-to-adr-workflow
/plugin install plan-to-adr-workflow@plan-to-adr-workflow
```

Verify installation:
```
/plugin list
```

## Commands

- `/commit-plan` — Save plan from current discussion
- `/implement-plan` — Execute saved plan

## When to Use

**Use for:**
- New features affecting multiple files
- Refactoring existing code
- Architectural changes
- Complex bug fixes

**Not needed for:**
- Minor fixes (typos, formatting)
- Single-file changes
- Quick fixes

## Workflow

### Step 1: Plan Mode

Press `Shift+Tab` or type `/plan` to enter planning mode.

Claude will:
1. **Explore** — Study codebase, find relevant files
2. **Clarify** — Ask questions about requirements
3. **Design** — Propose implementation approach
4. **Agree** — Discuss and refine the plan

### Step 2: /commit-plan

After agreeing on a plan, save it:

```
/commit-plan
```

Creates `docs/plans/active/YYYY-MM-DD_name.md` with:
- Context and Q&A from discussion
- Tasks breakdown
- Affected files
- Risks and consequences

### Step 3: /implement-plan

When ready to implement:

```
/implement-plan
```

Claude will:
1. Show available plans
2. Execute tasks sequentially
3. Mark progress (`[ ]` → `[x]`)
4. Run tests after each phase
5. Move completed plan to ADR

## Folder Structure

The plugin creates and uses this structure in your project:

```
docs/
├── plans/
│   ├── README.md     # Instructions
│   ├── STATUS.md     # Current status and roadmap
│   ├── active/       # Plans in progress
│   └── backlog/      # Deferred ideas
└── ADRs/
    └── YYYY-MM/      # Completed plans (Architecture Decision Records)
```

## Example: User Authentication Feature

```
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

## Plan Template

Each plan includes:

| Section | Description |
|---------|-------------|
| **Metadata** | Date, status, complexity, progress |
| **Context** | Why this decision is needed |
| **Solution** | What we decided + rejected alternatives |
| **Tasks** | Checklist by phases |
| **Affected Files** | What files will change |
| **Consequences** | Positive effects, risks, tech debt |
| **Testing** | How to verify it works |
| **Documentation** | What to update in docs |

## Language Support

- **Plan content**: Written in user's language (auto-detected)
- **Filenames**: Always English (kebab-case)

## ADR — Architecture Decision Records

Completed plans automatically become ADRs:
- **Memory** — Why decisions were made
- **Onboarding** — New devs understand history
- **Revision** — Review decisions with new context

## License

MIT