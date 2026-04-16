---
name: commit-adr
description: Create an ADR (Architecture Decision Record) directly from the current chat discussion — for design decisions without implementation, decisions made in-session, or retroactive documentation of completed work.
---

# Commit ADR

Creates an Architecture Decision Record from the current discussion, bypassing the plan → implement cycle.

**Use when:**
- A design decision has been made but won't be implemented right now.
- A decision was made and implemented in the same session without a plan.
- Past work was done without a plan and needs to be documented retroactively.
- A decision needs to be recorded for posterity independent of implementation.

## Process

1. **Analyze the discussion** — extract the decision, motivation, alternatives, consequences, and Q&A from the current conversation.
2. **Determine status**:
   - `Accepted` — decision is made and agreed upon.
   - `Proposed` — still under discussion, recorded for visibility.
   - `Superseded` — replaces an earlier ADR (note which one in References).
3. **Determine context type**:
   - `Design-only` — decision without implementation.
   - `Implemented` — decision already implemented in this session/project.
   - `Retroactive` — documenting past work that was done without a plan.
4. **Read the template** at `${SKILL_ROOT}/template.md` and fill it in.
5. **Save** to `docs/ADRs/YYYY-MM/YYYY-MM-DD_short-name.md`.
6. If `docs/ADRs/` doesn't exist — create the directory structure.

## Rules

- Filename: `YYYY-MM-DD_short-name.md` (kebab-case, English).
- Title prefix: `ADR:` (e.g., `# ADR: Use Postgres over MySQL for write-heavy workloads`).
- Content in the user's language.
- **Not** coupled to `docs/plans/` — this is an independent path.
- If the ADR supersedes another, fill the `References` section with a link to the old ADR.

## ADR Template

See `${SKILL_ROOT}/template.md`. Read this file before filling in the ADR.

## Language

- **Content**: User's language.
- **Filenames**: English, kebab-case.
- **Status / Context Type terms**: Keep in English.
- **Folder structure**: Keep in English (`docs/ADRs/YYYY-MM/`).

## Output

Return to the user:
- Path to the created ADR file.
- Brief summary: decision + status + context type.

## When NOT to Use

- If you are about to implement the decision as a multi-step task — use `/commit-plan` instead; `/implement-plan` will archive it as an ADR when finished.
- For trivial decisions that don't affect architecture — just make the change.
