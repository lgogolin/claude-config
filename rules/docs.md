# Documentation Discipline

Applies to every README, code comment, commit body and plan file.

## 1. Same Style As Output

Write docs the way the active output style tells you to talk. If the output
style is ELI5 / Simplified Technical English, the README is too: short
sentences, plain words, active voice, one idea per sentence.

A README is not where you get to be verbose again.

## 2. Length Is A Budget

Counted as prose. Fenced code blocks do not count — a runbook needs its
commands.

- Unit or component README: **80 prose lines max**.
- Project or repo README: **40 prose lines max**.
- Code comment: **3 lines max**.

Over budget means cut, not wrap. If it does not fit, the extra was not needed.

## 3. What A README Is For

Four questions only:

1. What is this?
2. How do I deploy or run it?
3. What do I check to know it works?
4. What is still broken or owed?

Nothing else earns a line.

## 4. What Never Goes In

- **Why we chose X over Y.** That is the commit message. A new reader does not
  care what was considered.
- **Migration or conversion narrative.** "Was listed as", "an early draft",
  "until 2026-05-01". Dead weight.
- **Restating a manifest.** If `replicas: 3` is in the file, do not write
  "three replicas, as the chart had".
- **Ticket or MR numbers.** Stale the moment they merge.
- **Warnings against approaches nobody proposed.** "Do not use a Service and
  Endpoints here" belongs in a review, not a README.

A real constraint that will bite someone is worth one sentence. The reasoning
behind it is not.

## 5. One Fact, One Home

State a fact in exactly one file. Cross-reference instead of repeating.

Repeating a fact in three places guarantees two of them go stale, and a
reviewer who spots the mismatch is right to fail the review.

## 6. Verify Every Command And Reference

Before a command or a name goes in a doc:

- Run the command, or render the manifest that defines the name.
- Check object names against the file that creates them, not against memory.
- Check that a claimed proof actually proves the claim. A readiness probe on
  `/` does not prove a database connection. An open TCP port does not prove a
  service answers.

An unverified command in a runbook is worse than no runbook.

## 7. Say What Is Not Done

Bugs, owed rotations, untested assumptions and manual steps go in a short
"What is still owed" list. Never bury them in prose.

---

**Working if:** a reviewer can read the whole file without losing track, and
every command in it runs.
