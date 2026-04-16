# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin that provides a structured development planning workflow with ADR (Architecture Decision Records) support. Two flows are supported:

```text
Plan Mode → /commit-plan → /implement-plan → ADR   (full cycle)
Chat      → /commit-adr                      → ADR  (direct ADR)
```

## Plugin Structure

```text
.claude-plugin/
  plugin.json                  # Plugin metadata (name, version, description, skills)
skills/
  commit-plan/
    SKILL.md                   # Save Plan Workflow
    template.md                # Plan template (referenced via ${SKILL_ROOT}/template.md)
  implement-plan/
    SKILL.md                   # Execute Plan Workflow
  commit-adr/
    SKILL.md                   # Direct ADR from chat
    template.md                # ADR template (referenced via ${SKILL_ROOT}/template.md)
```

Each skill that needs a template keeps it inside its own folder and references it via `${SKILL_ROOT}/template.md`. Edit templates without touching skill logic.

## Skills

- `/commit-plan` — Analyzes current discussion and creates a plan at `docs/plans/active/YYYY-MM-DD_name.md`.
- `/implement-plan` — Executes a saved plan, marks tasks complete, archives finished plans under `docs/ADRs/YYYY-MM/`.
- `/commit-adr` — Records an ADR directly from chat at `docs/ADRs/YYYY-MM/YYYY-MM-DD_name.md` (design-only, implemented, or retroactive).

## Key Files

- `skills/commit-plan/SKILL.md` — Save Plan Workflow.
- `skills/commit-plan/template.md` — Plan template.
- `skills/implement-plan/SKILL.md` — Execute Plan Workflow.
- `skills/commit-adr/SKILL.md` — Direct ADR Workflow.
- `skills/commit-adr/template.md` — ADR template.

## Conventions

- Plan / ADR filenames: `YYYY-MM-DD_short-name.md` (kebab-case, English).
- Status terms kept in English: `Planning`, `In Progress`, `Completed` (plans); `Accepted`, `Proposed`, `Superseded` (ADRs).
- Folder structure kept in English: `docs/plans/`, `docs/ADRs/`.
- Plan / ADR content written in the user's language.
- Templates are the source of truth — do not duplicate their content inside SKILL.md files.
- Inside SKILL.md reference templates via `${SKILL_ROOT}/template.md`, never relative paths.
