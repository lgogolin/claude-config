---
name: docs-update
description: Update local documentation to match current codebase after code changes.
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
  - Write
  - Bash(git diff *)
  - Bash(git log *)
---

# /docs-update — Keep Documentation Current

## Before Starting

1. Read `CLAUDE.md` to discover:
   - Where documentation lives (e.g., `docs/`, README files, API specs)
   - What kinds of docs the project maintains
   - Any documentation conventions

2. Read `.claude/plans/implementation-summary.md` to understand what changed recently.

## Workflow

1. **Scan recent changes** — use `git diff` and `git log` to identify what changed since docs were last updated.

2. **Discover documentation files:**
   - Look for `docs/` directory, README files, API specs, architecture docs
   - Check CLAUDE.md for documentation pointers
   - Use Glob to find `**/*.md`, `**/*.yaml`, `**/*.yml` in doc locations

3. **Compare code against docs** — identify what's stale or missing:
   - New components/modules not documented
   - Changed APIs or interfaces
   - Modified configuration or setup steps
   - New conventions or patterns introduced

4. **Update affected files** — edit docs to match current codebase state.

5. **Update CLAUDE.md if needed** — if component structure, build commands, or conventions changed.

## Rules

1. **Never remove documentation sections** — only update or add. Flag removals for human review.
2. **Preserve formatting** — match the existing markdown structure and style.
3. **Be specific** — use actual names, paths, and values from the code. No vague descriptions.
4. **Cross-reference** — when updating one doc, check if related docs need updates too.
