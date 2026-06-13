---
name: city-ledger
description: >
  The city's public record: a single file, .agent-city/ledger.md, committed on the
  feature branch — the work order, every phase transition, every failure routed, every
  decision made by convention. Makes the run auditable by the PR reviewer and resumable
  by a future session. Use at intake (open the ledger), at every phase transition
  (append), and on /phoenix in a repo with a ledger already present (resume).
---

# City Ledger

Autonomous runs die two deaths: the session ends mid-flight and the next one starts
over, or the PR lands on a reviewer's desk with no account of how it was made. One file
prevents both.

## The file

`.agent-city/ledger.md`, committed **on the feature branch** — it rides with the PR, so
the reviewer gets the full account, and it dies with the branch if the PR is rejected.
It never exists on the default branch unless a human merges it there knowingly.

```markdown
# Agent City Ledger
> machine-written run record — see the PR description for the human summary

## Work Order
<the intake skill's work order, verbatim>

## Phase Log
- [2026-06-11 14:02] intake — work order opened
- [2026-06-11 14:03] db-consult — Postgres confirmed, local; user answered 1 batch
- [2026-06-11 14:05] blueprint — 4 increments planned; persistence: provisioned DB
- [2026-06-11 14:11] construct — increments 1-4 built; 1 deviation (see Decisions)
- [2026-06-11 14:14] test-run — FAIL 1/12: redirect strips trailing slash → construct
- [2026-06-11 14:17] test-run — PASS 12/12 (iteration 1 of 5)
- [2026-06-11 14:18] doc-sync — README + CHANGELOG updated (2 endpoints, usage example)
- [2026-06-11 14:18] budget — ~310k tok run (est); blueprint reused, no re-reads flagged
- [2026-06-11 14:19] pr-open — PR #42 opened
- [2026-06-11 14:25] pr-steward — CI green; awaiting review

## Decisions (made by convention, disclosed here)
- chose pytest; repo had no test framework
- slugs are base62, 7 chars — work order didn't specify

## Status
phase: pr-steward · iteration: 1/5 · PR: #42 · blocked-on: nothing
```

## Rules

- **Opened by intake** (work order verbatim), **appended at every phase transition** —
  one line each: timestamp, phase, outcome. Failures log their diagnosis in the line.
- **The Status block is always current.** It's the resume point and the `/city-status`
  answer; a stale Status is a ledger defect.
- **Entries are facts, not narrative.** Real test counts, real PR numbers, the actual
  diagnosis. The ledger inherits the city's honesty laws — it never records "passing"
  that wasn't observed.
- **Never a secret in the ledger.** It's committed; connection strings, tokens, and
  anything credential-shaped stay out. Env var *names* are fine.
- **Opt-out:** a work order that says "no ledger" skips the file entirely; the final
  report carries the disclosure load alone.

## Resume protocol

On `/phoenix` (or any pipeline start) in a repo where the current branch has a ledger:

1. Read Status — it names the phase in flight and the iteration count.
2. Verify against reality before trusting it: does the branch exist as recorded, does
   the PR exist, what do `gh pr checks` actually say. Reality wins every disagreement.
3. Continue from the verified phase. Completed phases are never re-run; a phase that was
   mid-flight restarts cleanly from its own beginning.
4. Log the resume: `- [ts] resume — session N picking up at <phase>`.

## Guardrail

The ledger records the run; it never drives it falsely. If the ledger and the world
disagree, the world is right, the disagreement gets logged, and the run continues from
what's real.
