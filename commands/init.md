# Project Initialization

You are guiding the user through setting up their project for the agent workflow (architect → implementer → reviewer).

## Step 1: Check existing state

- Check if `CLAUDE.md` already exists (check both `.claude/CLAUDE.md` and the project root). If it does, ask the user if they want to update it or start fresh.
- Read the template at `~/.claude/templates/CLAUDE.md.template` for the target structure.
- Explore the project directory (use Glob and Grep) to pre-discover what you can: languages, frameworks, directory structure, existing build files (Makefile, package.json, Cargo.toml, CMakeLists.txt, pubspec.yaml, go.mod, etc.).
- If a `go.mod` is found, also read `~/.claude/templates/go-microservice-CLAUDE.md.template` and use it as the primary template instead of the generic one. Adapt its sections (file structure, patterns, test commands, env vars) to match the actual project.
- If a `next.config.*` or `package.json` with `next` dependency is found, read `~/.claude/templates/nextjs-frontend-CLAUDE.md.template` and use it as the primary template. Adapt for the actual project's auth, database, and UI choices.

## Step 2: Present what you found

**If the project directory is empty or has no recognizable config files** (no go.mod, package.json, Cargo.toml, etc.):
- List available templates from `~/.claude/templates/`:
  - `CLAUDE.md.template` — generic project
  - `go-microservice-CLAUDE.md.template` — Go microservice
  - `nextjs-frontend-CLAUDE.md.template` — Next.js frontend
- Ask: **"This looks like a fresh project. What type of project are you starting?"** and let the user pick a template or describe their stack.
- Use the selected template as the base and proceed to Step 3 for customization.

**If the project has existing files:**
Show the user what you auto-detected:
- Project language(s) and framework(s)
- Directory structure / likely components
- Build system(s) found
- Any existing docs

Then ask: **"Does this look right? What am I missing or getting wrong?"**

## Step 3: Ask targeted questions

Only ask what you couldn't auto-detect. Skip questions you already have answers to. Ask one group at a time, not all at once.

**Group 1 — Overview:**
- What does this project do? (one paragraph)
- Is there anything unusual about the architecture?

**Group 2 — Components** (if not obvious from structure):
- What are the main components and how do they relate?
- Are there shared/generated code dependencies between them?

**Group 3 — Build & Verify:**
- What are the build commands for each component?
- What test/lint commands should agents run to verify changes?
- Any preferred tools (MCP tools, specific CLI tools) over defaults?

**Group 4 — Conventions:**
- Any architectural patterns agents must follow? (e.g., state management, error handling, naming)
- Any anti-patterns to avoid? (things agents might try that would be wrong here)
- Where are the important docs?

## Step 4: Generate CLAUDE.md

Based on answers + auto-detection, generate `.claude/CLAUDE.md` following the template structure. Always place the file inside the `.claude/` directory, not in the project root. Include:
- Project Overview
- Components table (path, stack, purpose)
- Build Commands for each component
- Documentation pointers
- Conventions and patterns
- The agent workflow sections (these come from the global `~/.claude/CLAUDE.md` but can be customized)

Write the file to `.claude/CLAUDE.md` and show the user what was generated.

## Step 5: Scaffold agent directories

Create the directory structure agents expect:
```
.claude/plans/          — active-plan.md and implementation-summary.md go here
.claude/plans/archive/  — completed plans
.claude/memory/         — decisions.md and scratchpad.md
```

Create empty starter files:
- `.claude/memory/decisions.md` with a header and empty table
- `.claude/plans/implementation-summary.md` with a "No previous work" note

## Step 6: Generate domain guide skills

For each major tech stack detected, offer to create a domain guide skill in `.claude/skills/`. These are non-user-invocable skills that agents load for project-specific coding conventions.

Each domain guide should follow this structure:
```markdown
---
name: <stack>-guide
description: <Stack> conventions for this project.
user-invocable: false
disable-model-invocation: true
---

# <Stack> Guide — Conventions

## Scope
Which directories/files this covers.

## Reference Docs
Key docs to read for this part of the codebase.

## Build Environment
Setup steps and build commands.

## Code Conventions
Numbered list of project-specific patterns:
- State management approach
- File/directory organization
- Naming conventions
- Preferred tools (MCP tools, linters, etc.)
- Anti-patterns to avoid

## Quality Criteria
What "done right" looks like for this stack.
```

Ask the user about conventions for each stack — this is where project-specific knowledge lives that agents can't auto-detect.

If domain guide skills are created, ask the user if they want project-level agent overrides in `.claude/agents/` that reference them via `skills:` in frontmatter. If not, the global agents work fine without them — they just won't have deep stack-specific guidance.

## Rules

- Be conversational, not interrogative. One group of questions at a time.
- Use auto-detection to minimize questions. Don't ask what you can see.
- If the user gives short answers, that's fine — fill in reasonable defaults and confirm.
- The output should be a CLAUDE.md that lets the global agents (architect, implementer, reviewer) work effectively on this project without any project-specific agent overrides.
- Domain guide skills are optional — only create them if the project has conventions worth capturing beyond what's in CLAUDE.md.
