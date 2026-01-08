---
name: plan-to-adr-workflow
description: Manages development plans. Creates plans from discussions (/commit-plan), executes saved plans (/implement-plan), archives to ADR. For complex features, refactoring, architectural changes.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git:*), Task
---

# Plan to ADR Workflow

Skill for managing development plans with ADR (Architecture Decision Records) support.

## Process Overview

```
Plan Mode → /commit-plan → /implement-plan → ADR
```

## Commands

- `/commit-plan` — save plan from discussion
- `/implement-plan` — execute saved plan

---

## /commit-plan — Save Plan

Analyzes current discussion and creates a development plan.

### Process

1. Analyze current session
2. Extract context, tasks, Q&A from discussion
3. Create plan using template from `skills/plan-to-adr-workflow/TEMPLATE.md`
4. Save to `docs/plans/active/YYYY-MM-DD_name.md`
5. Update `docs/plans/STATUS.md` — add to active plans table

### Rules

- If questions were asked — include Q&A in "Questions and Answers" section
- All tasks must be uncompleted: `[ ]`
- Filename: `YYYY-MM-DD_short-name.md` (kebab-case, English)
- Progress: `0%`, Status: `Planning`
- Do NOT execute the plan!

### Output

Return to user:
- Path to created plan file
- Brief summary (goal, number of tasks)

---

## /implement-plan — Execute Plan

Executes a saved development plan.

### Process

1. **Plan Selection:**
   - If plan was created in current dialog — execute it
   - Otherwise:
     - Show filenames from `docs/plans/active/` and `docs/plans/backlog/`
     - Ask user: "Which plan do you want to execute?"
     - Wait for selection
     - Read selected file

2. **Execution:**
   - Execute tasks sequentially
   - After each task: replace `[ ]` with `[x]` in plan file
   - Update progress in metadata
   - Run tests after each phase

3. **Completion:**
   - Set status: `Completed`, progress: `100%`
   - Update `docs/plans/STATUS.md` — remove from active, add to history
   - Move to ADR:
     ```bash
     mkdir -p docs/ADRs/YYYY-MM
     mv docs/plans/active/PLAN.md docs/ADRs/YYYY-MM/
     ```
   - Add `ADR: ` prefix to document title
   - Call `/update-changelog` if available

### Output

Return to user:
- List of completed tasks
- Path to ADR
- What was updated (CHANGELOG, STATUS)

---

## Language

- **Plan content**: Write in the user's language (the language they use in messages)
- **Filenames**: Always English, kebab-case (`2025-01-08_feature-name.md`)
- **Status terms**: Keep in English (Planning, In Progress, Completed)
- **Folder structure**: Keep in English (docs/plans/, docs/ADRs/)
- **Template sections**: Translate to user's language when creating plan

---

## Project Initialization

If `docs/plans/` structure doesn't exist, create it:

```
docs/
├── plans/
│   ├── README.md
│   ├── STATUS.md
│   ├── active/
│   └── backlog/
└── ADRs/
```

---

## Plan Template

Use template from `skills/plan-to-adr-workflow/TEMPLATE.md`.

---

## Plan Statuses

| Status | Description |
|--------|-------------|
| `Planning` | Plan created, tasks being refined |
| `In Progress` | Being executed |
| `Completed` | Finished, ready to move to ADR |

---

## When to Use

**Use for:**
- New features affecting multiple files
- Refactoring existing code
- Architectural changes
- Fixing complex bugs

**Not needed for:**
- Minor fixes (typos, formatting)
- Single-file changes
- Quick fixes