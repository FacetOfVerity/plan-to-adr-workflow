---
name: commit-plan
description: Save development plan from current discussion to docs/plans/active/. Use after plan-mode brainstorming when ready to commit the agreed approach to a file.
---

# Commit Plan

Analyzes the current discussion and saves it as a development plan in `docs/plans/active/`.

## Process

1. Analyze the current session.
2. Extract context, tasks, and Q&A from the discussion.
3. Read the template at `${SKILL_ROOT}/template.md` and fill it in.
4. Save to `docs/plans/active/YYYY-MM-DD_short-name.md`.
5. Update `docs/plans/STATUS.md` — add to the active plans table.

## Rules

- If questions were asked — include Q&A in the "Questions and Answers" section.
- All tasks must be uncompleted: `[ ]`.
- Filename: `YYYY-MM-DD_short-name.md` (kebab-case, English).
- Initial metadata: `Status: Planning`, `Progress: 0%`.
- Do **NOT** execute the plan — this skill only saves it.

## Plan Template

See `${SKILL_ROOT}/template.md`. Read this file before filling in the plan.

## Language

- **Plan content**: Write in the user's language (the language they use in messages).
- **Filenames**: Always English, kebab-case (`2025-01-08_feature-name.md`).
- **Status terms**: Keep in English (`Planning`, `In Progress`, `Completed`).
- **Folder structure**: Keep in English (`docs/plans/`, `docs/ADRs/`).
- **Template sections**: Translate to the user's language when creating the plan.

## Project Initialization

If `docs/plans/` doesn't exist, create the structure:

```
docs/
├── plans/
│   ├── README.md
│   ├── STATUS.md
│   ├── active/
│   └── backlog/
└── ADRs/
```

## Output

Return to the user:
- Path to the created plan file.
- Brief summary (goal, number of tasks).

## When to Use

**Use for:**
- New features affecting multiple files.
- Refactoring existing code.
- Architectural changes.
- Complex bug fixes.

**Not needed for:**
- Minor fixes (typos, formatting).
- Single-file changes.
- Quick fixes.
