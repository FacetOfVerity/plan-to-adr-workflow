# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin that provides a structured development planning workflow with ADR (Architecture Decision Records) support. The workflow is:

```
Plan Mode → /commit-plan → /implement-plan → ADR
```

## Plugin Structure

```
.claude-plugin/
  plugin.json          # Plugin metadata (name, version, description)
commands/
  commit-plan.md       # Command definition (delegates to SKILL.md)
  implement-plan.md    # Command definition (delegates to SKILL.md)
skills/
  plan-to-adr-workflow/
    SKILL.md           # Main workflow logic and instructions
    TEMPLATE.md        # Plan document template
```

## Commands

- `/commit-plan` - Analyzes current discussion and creates a plan file at `docs/plans/active/YYYY-MM-DD_name.md`
- `/implement-plan` - Executes a saved plan, marks tasks complete, moves finished plans to `docs/ADRs/`

## Key Files

- `skills/plan-to-adr-workflow/SKILL.md` - Contains the complete workflow logic for both commands
- `skills/plan-to-adr-workflow/TEMPLATE.md` - Template used when creating new plans

## Conventions

- Plan filenames: `YYYY-MM-DD_short-name.md` (kebab-case, English)
- Status terms kept in English: Planning, In Progress, Completed
- Folder structure kept in English: docs/plans/, docs/ADRs/
- Plan content written in user's language
