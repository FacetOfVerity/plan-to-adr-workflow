---
name: implement-plan
description: Execute a saved development plan from docs/plans/, track task progress, and archive the completed plan as an ADR.
---

# Implement Plan

Executes a saved development plan, tracks progress in-place, and archives the finished plan as an ADR.

## Process

### 1. Plan Selection

- If a plan was created in the current dialog — execute it.
- Otherwise:
  - Show filenames from `docs/plans/active/` and `docs/plans/backlog/`.
  - Ask the user: "Which plan do you want to execute?"
  - Wait for selection.
  - Read the selected file.

### 2. Execution

- Execute tasks sequentially.
- After each task: replace `[ ]` with `[x]` in the plan file.
- Update `Progress` in the metadata.
- Run tests after each phase.

### 3. Completion

- Set `Status: Completed`, `Progress: 100%`.
- Update `docs/plans/STATUS.md` — remove from active, add to history.
- Move to ADR:
  ```bash
  mkdir -p docs/ADRs/YYYY-MM
  mv docs/plans/active/PLAN.md docs/ADRs/YYYY-MM/
  ```
- Add `ADR: ` prefix to the document title.
- Call `/update-changelog` if available.

## Plan Statuses

| Status | Description |
|--------|-------------|
| `Planning` | Plan created, tasks being refined |
| `In Progress` | Being executed |
| `Completed` | Finished, ready to move to ADR |

## Language

- **Content edits**: Preserve the plan's original language.
- **Status terms**: Keep in English.
- **Folder structure**: Keep in English (`docs/plans/`, `docs/ADRs/`).

## Output

Return to the user:
- List of completed tasks.
- Path to the archived ADR.
- What was updated (CHANGELOG, STATUS).
