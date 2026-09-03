---
name: pr-workflow
description: Use when creating a pull request - covers analyzing full commit history, drafting the PR summary, and pushing the branch.
---

# Pull Request Workflow

When creating PRs:
1. Analyze full commit history (not just latest commit)
2. Use `git diff [base-branch]...HEAD` to see all changes
3. Draft comprehensive PR summary
4. Include test plan with TODOs
5. Push with `-u` flag if new branch

Commit message conventions live in `rules/git.md`.
