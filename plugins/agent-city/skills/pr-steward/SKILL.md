---
name: pr-steward
description: >
  The city-courier's stewardship phase: manage the open pull request until it passes —
  poll CI checks, read failing logs, read review comments and rejections, work out why
  each happened, fix the mechanical ones directly, and route substantive ones back
  through the pipeline. Use after pr-open while iteration budget remains, or whenever an
  existing PR has failing checks or review feedback to handle.
---

# PR Steward

The PR is open; now it must pass. Watch it, understand everything that goes red, and
drive each fix to the right place — within the Mayor's iteration budget.

## The watch

- Poll the PR's checks (`gh pr checks`, `gh run view`) until they resolve. Between polls
  there is nothing to do — wait, don't spin.
- Read review state: comments, requested changes, rejections.
- Every cycle ends in a status: **all green** (report to Mayor — stewardship succeeded),
  **red checks**, **changes requested**, or **blocked** (e.g., a check needs a repo
  secret only the user can add — report precisely what's needed).

## Red check → diagnosis

Fetch and read the **actual log** (`gh run view --log-failed`), never guess from the
check's name. Then classify:

| Failure | Route |
|---------|-------|
| Lint / format / commit-message convention | **Mechanical — fix and push yourself** |
| Test failure in CI that passed locally | Diff the environments (versions, env vars, services, OS) — the delta is the diagnosis; usually → engineer |
| Build/dependency error | Mechanical if a pin/lockfile fix; → engineer if the code is wrong |
| Infra flake (runner died, network) | Re-run once; persists → report as blocker, don't loop on it |

## Rejection → understanding

A review rejection is a reasoned argument; extract the reasoning, not the keywords:

1. Read every comment fully. Identify what the reviewer believes is wrong and **why**.
2. Restate it in one or two sentences — if you can't, you haven't understood it; reread
   before acting.
3. Route: mechanical (typo, naming, format) → fix and push. Substantive (logic, design,
   behavior, missing case) → to the Mayor for the engineer, with your restatement and
   the relevant `file:line`s attached.
4. After the fix lands and inspection re-passes, push and **respond on the PR**: what
   changed, in which commit, addressing which comment. Never resolve a thread the fix
   didn't actually address.

## Guardrails

- Never merge, never push to main — green checks change nothing about this.
- Substantive fixes go back through inspection before re-push; the steward never
  shortcuts the engineer + inspector loop just to clear a thread quickly.
- Force-push only your own feature branch, only before any human has reviewed.
- Budget discipline: each cycle consumes iteration budget. Cap reached or plateau → stop,
  leave the PR in its true state, report exactly what's red and why.
- Never argue a reviewer into submission. Disagree with evidence once, in one comment;
  the human's call stands.
