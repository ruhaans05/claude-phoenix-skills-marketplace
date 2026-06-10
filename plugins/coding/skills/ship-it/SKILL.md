---
name: ship-it
description: >
  Gate a change before it lands: self-review the diff, run tests + lint/build, write a clear
  commit message and PR, then push — on a branch, only when the user asks. Catches problems
  the author missed before they reach review. Use when a change is done, or the user says
  "commit this", "open a PR", "get this ready to ship", "prep for review".
---

# Ship It

The last mile is where avoidable problems leak through: a stray debug log, a failing test, a
vague commit message. This skill is the pre-landing gate — review and verify before, not
after, the change is public.

## The gate (in order)

1. **Read your own diff.** `git diff` the full change. Look for: leftover debug prints/scratch
   code, commented-out blocks, TODOs you meant to resolve, secrets/keys, unrelated changes
   that slipped in, and anything that doesn't match the file's style.
2. **Run the checks.** Tests, linter, type-checker, build — whatever the project uses. Read
   the real output. Green before proceeding. If something fails, fix it or stop and report.
3. **Write the commit.** Conventional, imperative subject ≤ ~50 chars
   (`fix: token expiry off-by-one`). Body only when the *why* isn't obvious from the diff —
   explain intent and tradeoffs, not a line-by-line restatement.
4. **Open the PR** (if asked). Title = what changed; body = problem, approach, how it was
   verified (tests run, manual checks). Keep it scannable.

## Branch & push discipline

- Commit or push **only when the user asks.** Don't auto-push.
- If on the default branch (`main`/`master`), **create a feature branch first** — never
  commit a feature straight to the default branch.
- Use `gh` for PRs. Confirm the diff and target branch before opening.

## Commit message rules

- Subject: imperative mood, ≤ ~50 chars, no trailing period.
- Body: wrap ~72 cols, explain *why* when non-obvious; omit when the change is self-evident.
- One logical change per commit. Don't bundle a refactor + a feature + a fix.

## Guardrails

- **Never push to land while tests are red.** Report the failure with output instead.
- Don't commit secrets, `.env` values, or large generated artifacts — check the diff.
- Don't expand scope at the gate; if you spot an unrelated bug, note it, don't fix it here.
- Report outcome faithfully: what you ran, what passed, what you skipped and why.
- Pushing/PR-opening is outward-facing and hard to reverse — confirm before doing it unless
  the user already told you to proceed.
