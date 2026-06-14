# Global Instructions

## Agent Workflow

This workspace uses a plan-execute-review agent loop. Follow this workflow for non-trivial features:

1. **architect** — explores codebase, writes plan to `.claude/plans/active-plan.md`
2. **implementer** — reads the plan and executes it, writes results to `.claude/plans/implementation-summary.md`
3. **reviewer** — examines changes against plan and conventions, returns PASS or ISSUES
4. Loop steps 2–3 until reviewer returns PASS

## Core Directives

1. **Cross-domain awareness** — read `.claude/plans/implementation-summary.md` before starting work. A previous agent may have left context about related changes.
2. **Architecture decisions are recorded** — check `.claude/memory/decisions.md` before proposing alternatives to established patterns.
3. **Coding discipline** — follow [rules/common-discipline.md](./rules/common-discipline.md): think before coding, simplicity first, surgical changes, goal-driven execution.

## Routing

| Artifact | Location |
|----------|----------|
| Current work plan | `.claude/plans/active-plan.md` |
| Agent handoff summaries | `.claude/plans/implementation-summary.md` |
| Architecture decisions | `.claude/memory/decisions.md` |
| Shared scratchpad | `.claude/memory/scratchpad.md` |
| Agent activity log | `.claude/plans/agent-activity.log` |
| Completed plans | `.claude/plans/archive/` |

## Role-Based Agents

| Agent | Role | Scope |
|-------|------|-------|
| `architect` | Plan and design features. Read-only. | Writes to `active-plan.md` |
| `implementer` | Execute plans across all components. | Full tool access |
| `reviewer` | Review code for quality and conventions. Read-only. | Returns feedback |
