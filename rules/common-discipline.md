# Coding Discipline

Behavioral guidelines that apply to all agents (architect, implementer, reviewer) and all projects. Bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

Don't assume. Don't hide confusion. Surface tradeoffs.

Before implementing:
- State assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

Applies most strongly to **architect**: a plan written on a bad premise wastes the whole loop.

## 2. Simplicity First

Minimum code that solves the problem. Nothing speculative.

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Test: would a senior engineer say this is overcomplicated? If yes, simplify.

## 3. Surgical Changes

Touch only what you must. Clean up only your own mess.

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

Every changed line should trace directly to the user's request.

Applies most strongly to **implementer**: full tool access invites drive-by refactor.

## 4. Goal-Driven Execution

Define success criteria. Loop until verified.

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
```

Strong criteria let the agent loop independently. Weak criteria ("make it work") require constant clarification.

Applies most strongly to **reviewer**: PASS/ISSUES means little without explicit criteria to check against.

---

**Working if:** fewer unnecessary diffs, fewer rewrites from overcomplication, clarifying questions come before implementation rather than after mistakes.

> Adapted from [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) CLAUDE.md.
