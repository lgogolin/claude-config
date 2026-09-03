# Git Conventions

## Branch Safety

- **Never commit directly to `main` or `master`**. Always work on a feature branch.
- Before making any changes, check the current branch with `git branch --show-current`. If on `main` or `master`, create and switch to a new branch first.
- If uncommitted changes already exist on `main`/`master`, stash them, create a branch, then unstash.

## Branch Naming

- `feature/<description>` — new functionality
- `fix/<description>` — bug fixes
- `chore/<description>` — maintenance, refactoring, config changes
- `docs/<description>` — documentation only
- Use kebab-case for descriptions: `feature/add-user-auth`, `fix/null-pointer-on-login`
- Include ticket number when available: `feature/PROJ-123-add-user-auth`

## Commit Messages

Format: `<type>(<component>): <description>`

```
feat(api): add user registration endpoint
fix(auth): handle expired token refresh
chore(deps): upgrade express to v5
docs(readme): add deployment instructions
refactor(db): extract query builder helper
test(auth): add login edge case coverage
```

### Types
- `feat` — new feature
- `fix` — bug fix
- `refactor` — code change that neither fixes a bug nor adds a feature
- `chore` — maintenance (deps, config, CI)
- `docs` — documentation only
- `test` — adding or updating tests
- `style` — formatting, whitespace (no logic change)

### Rules
- Component name should match the project's component structure (from CLAUDE.md if available)
- Description in imperative mood, lowercase, no period at the end
- Keep the first line under 72 characters
- Keep it minimal. Body only when the *why* isn't obvious from the subject — no summaries of what the diff already shows
- No attribution trailers, no tool footers, no emoji (`Co-Authored-By`, `Generated with ...`). Enforced by `"includeCoAuthoredBy": false` in `~/.claude/settings.json`
