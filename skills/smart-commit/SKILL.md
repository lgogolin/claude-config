---
name: commit
description: Analyzes changes across project components and creates properly scoped commits.
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - Bash(git status *)
  - Bash(git diff *)
  - Bash(git add *)
  - Bash(git commit *)
  - Bash(diff *)
  - Read
  - Glob
---

# /commit — Smart Component-Scoped Commit

Detect changes across all project components and create properly scoped commits.

## Before Starting

Read `CLAUDE.md` to understand the project's component structure and any commit conventions.

## Workflow

1. **Discover project structure:**
   - Read `CLAUDE.md` for components table (paths, names)
   - If multiple git repos, scan each for changes
   - If monorepo, determine component boundaries from directory structure

2. **Scan for changes:**
   ```bash
   git status --short
   ```
   For multi-repo setups, check each repo separately.

3. **Determine scope prefix:**
   - Changes in a single component → `[component-name]`
   - Changes across multiple components → `[multi]`
   - Changes only in docs/config → `[docs]`
   - Derive component names from directory structure or CLAUDE.md

4. **Shared/generated code safety check:**
   - If shared files were changed, verify consistency across consumers
   - If generated files were changed, warn that regeneration may be needed

5. **Show summary** of all changes before committing.

6. **Ask for confirmation** before each commit. Suggest a commit message with the scope prefix.

7. **Commit** — for multi-repo setups, commit each repo separately.

## Rules
- Never auto-push — commits are local only
- One commit per repo (don't split changes within a repo)
- Commit message format: `[scope] Imperative description of change`
- If CLAUDE.md defines commit conventions, follow those instead
