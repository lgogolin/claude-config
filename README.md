# Claude Code — Portable Config

Version-controlled `~/.claude` configuration: agents, commands, skills, rules, templates, and settings. Sync the same workflow across machines.

## What's tracked

| Path | Purpose |
|------|---------|
| `CLAUDE.md` | Global instructions injected into every session |
| `settings.json` | Hooks, enabled plugins, theme, effort level |
| `settings.local.json` | Per-machine permission allowlist |
| `agents/` | Role-based subagents (architect, implementer, reviewer) |
| `commands/` | Slash commands (e.g. `/init`) |
| `skills/` | Skill packs (the `learned/` subdir is gitignored) |
| `rules/` | Cross-project coding rules (git, security, golang, …) |
| `templates/` | Starter `CLAUDE.md` templates per stack |

## What's ignored

Session state, cache, telemetry, OAuth credentials, project memory, paste cache, learned skills, plans. See `.gitignore`.

## Agent workflow

Plan → execute → review loop. Defined in `CLAUDE.md`.

1. `architect` — read-only. Explores codebase, writes `.claude/plans/active-plan.md`.
2. `implementer` — reads plan, writes code, writes `.claude/plans/implementation-summary.md`.
3. `reviewer` — read-only. Returns PASS or ISSUES.
4. Loop 2–3 until PASS.

Routing table:

| Artifact | Location |
|----------|----------|
| Active plan | `.claude/plans/active-plan.md` |
| Handoff summary | `.claude/plans/implementation-summary.md` |
| Architecture decisions | `.claude/memory/decisions.md` |
| Scratchpad | `.claude/memory/scratchpad.md` |
| Agent activity log | `.claude/plans/agent-activity.log` |
| Archived plans | `.claude/plans/archive/` |

## Agents

| Agent | Role | Tools |
|-------|------|-------|
| `architect` | Plan/design features | Read, Grep, Glob, Write, WebFetch, WebSearch |
| `implementer` | Execute plans | Full tool access |
| `reviewer` | Review changes | Read-only |

## Commands

| Command | Purpose |
|---------|---------|
| `/init` | Walk through project setup: detect stack, generate `.claude/CLAUDE.md`, scaffold `plans/` and `memory/`, optionally add domain-guide skills |

## Skills

Local skills under `skills/`:

- `api-design` — REST design patterns
- `architecture-decision-records` — capture ADRs from sessions
- `clickhouse-io` — ClickHouse query/data engineering patterns
- `context-budget` — audit context window usage
- `continuous-learning` — extract reusable patterns into learned skills (Stop hook)
- `database-migrations` — migration best practices
- `deep-research` — multi-source research via firecrawl + exa
- `design-system` — generate/audit design systems
- `docker-patterns` — Docker / Compose patterns
- `docs-update` — update local docs after code changes
- `e2e-testing` — Playwright E2E patterns
- `frontend-patterns` — React / Next.js patterns
- `nextjs-turbopack` — Next.js 16+ and Turbopack
- `postgres-patterns` — PostgreSQL query/schema patterns
- `security-scan` — scan `.claude/` for misconfig and injection
- `siderolabs` — Talos/Omni Kubernetes ops
- `smart-commit` — scoped commits across components
- `strategic-compact` — suggest compaction (PreToolUse hook)

`skills/learned/` is gitignored — generated per machine by `continuous-learning`.

## Plugins

Enabled in `settings.json`:

- `context7@claude-plugins-official` — fetch current library docs via MCP
- `claude-md-management@claude-plugins-official` — audit/improve `CLAUDE.md`
- `gopls-lsp@claude-plugins-official` — Go LSP
- `warp@claude-code-warp` — Warp terminal integration
- `caveman@caveman` — ultra-compressed communication mode

Extra marketplaces: `warpdotdev/claude-code-warp`, `JuliusBrussee/caveman`.

## Hooks

Configured in `settings.json`:

| Event | Matcher | Action |
|-------|---------|--------|
| `PreToolUse` | `Edit\|Write` | `skills/strategic-compact/suggest-compact.sh` |
| `Stop` | — | `skills/continuous-learning/evaluate-session.sh` |

## Rules

Cross-project guardrails under `rules/`:

- `common-git-workflow.md` — commit format, PR workflow
- `git.md` — branch safety, naming, commit conventions
- `common-security.md` — secret management, pre-commit checks
- `golang/` — formatting, hooks, patterns, security, testing

## Templates

Starter `CLAUDE.md` templates used by `/init`:

- `CLAUDE.md.template` — generic
- `go-microservice-CLAUDE.md.template`
- `nextjs-frontend-CLAUDE.md.template`

## Sync to another machine

```sh
git clone <this-repo> ~/.claude
```

Then run Claude Code. `settings.local.json` and `skills/learned/` populate locally and stay out of git.

## Security

- `.credentials.json` and `~/.claude.json` are gitignored — never commit OAuth tokens or anonymous IDs.
- Run the `security-scan` skill periodically to check `.claude/` for injection risks and misconfig.
