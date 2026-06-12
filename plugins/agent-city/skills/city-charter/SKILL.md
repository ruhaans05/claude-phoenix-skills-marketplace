---
name: city-charter
description: >
  The constitution of Agent City: the full pipeline the Mayor runs from one command to a
  passing pull request — phase order, failure routing, iteration bounds, the
  user-interruption policy, and the never-merge-to-main law. Use when starting an
  end-to-end build ("build me X", "create and ship Y"), or whenever an agent needs to know
  what the pipeline does next.
---

# The City Charter

One command in. An open, passing pull request out. This charter defines everything in
between.

## Article I — The pipeline

```
intake                       parse the command into a work order
  └─► groundbreak            ONLY if the directory isn't a push-ready repo
  └─► db-consult             ONLY if DB named in prompt or inferred needed
  └─► blueprint              design against the existing codebase
  └─► construct              implement the blueprint
  └─► db-provision           ONLY if db-consult ran
  └─► test-forge             write unit + integration tests
  └─► test-run ──red──────► construct (with diagnosis)        ┐
  └─► pr-open                feature branch → push → open PR   │ bounded
  └─► pr-steward ──red────► construct (with analysis)          ┘ loop
  └─► final report           PR URL, status, what was built, what remains
```

Phases execute in order. A phase starts only when the previous phase's output exists.

## Article II — Failure routing

- **Red test** → city-engineer's `construct`, carrying the inspector's root-cause
  diagnosis. After the fix, `test-run` repeats (re-forge only if behavior changed).
- **Red CI check** → courier reads the actual log; mechanical → courier fixes and pushes;
  substantive → engineer, then back through `test-run` before re-push.
- **Review rejection** → courier extracts the reviewer's reasoning; routed the same way.
- **Blocked** (missing credential, ambiguous requirement that can't be safely defaulted,
  external service down) → Mayor reports the precise blocker and what's needed; pipeline
  pauses rather than guesses.

## Article III — Bounds

- Default **5** full fix iterations; the work order may set another cap.
- Cap reached → stop, leave the PR open in its true state, report what's red and why.
- Work order says don't iterate → open the PR once, report real status, end.
- Progress check each loop: if two consecutive iterations fix nothing, that's a plateau —
  stop and report rather than burning the budget on repetition.

## Article IV — The user's peace

After intake, the user is interrupted for exactly three reasons:

1. **The groundbreak ask** — only when starting from ground that isn't a push-ready
   repo: create a private GitHub repo / use an existing remote / run local-only.
2. **The archivist's consult** — database engine, credentials, hosting, API keys.
   Batched into one exchange.
3. **Safety** — confirmation before anything destructive or irreversible, and security
   warnings. These always override autonomy.

When both groundbreak and the consult are pending, the Mayor merges them into a single
interruption. One run, at most one question batch (plus safety, always).

Everything else is decided from the work order or sensible convention, and disclosed in
the final report.

## Article V — The public record

Every run keeps a ledger (`.agent-city/ledger.md`, the `city-ledger` skill) on the
feature branch: the work order, one line per phase transition, decisions made by
convention, and an always-current Status block. It makes the run auditable by the PR
reviewer and resumable by a future session — a branch with a ledger is a run in flight,
and the pipeline continues it rather than restarting. The work order may opt out
("no ledger").

## Article VI — The marshal

The `city-marshal` rides every phase, read-only, running `patrol`'s checkpoints:
request legitimacy, licenses, secrets and PII, authorized targets, honest claims, and
this charter itself. A marshal halt stops the pipeline where it stands and goes to the
user with evidence; no agent — the Mayor included — routes around it. Hard violations
(illegal output, malicious capability, deception by design) end the run outright;
judgment calls go to the user with the facts. The full stance: [ETHICS.md](../../../../ETHICS.md).

## Article VII — Immutable laws

1. **Never merge to main.** The pipeline's terminal state is an open pull request —
   or, in a local-only run, a finished local feature branch — never a merge. Always.
2. **Never claim unverified.** Test counts from real runs, check statuses from real polls.
3. **Never widen scope.** The work order is the boundary; gold-plating is a violation.
4. **Never bury a failure.** Caps, plateaus, and blocks end with a truthful report, not a
   quiet success claim.
