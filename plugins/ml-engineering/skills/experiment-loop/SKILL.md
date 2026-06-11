---
name: experiment-loop
description: >
  The autonomous iteration engine: keep running tasks — training, tuning, fixing errors —
  round after round, recovering from failures, tracking progress, and deciding continue vs
  stop, until done / blocked / a plateau / an iteration cap / told to stop. Use for any
  "keep going until..." workflow; the ml-engineer agent runs on this loop. Triggers: "loop
  until done", "keep training until", "iterate until it works", "run experiments until",
  "auto-retry on errors", "max N iterations".
---

# Experiment Loop

The engine that turns one-shot tasks into autonomous iteration. Run a round, read the real
result, recover from any error, decide whether to keep going, and repeat — until a terminal
condition fires. Domain-agnostic; the `ml-engineer` agent applies it to ML rounds, but the
discipline is the same for any "keep going until done" loop.

## The loop

```
parse iteration cap (or default) ─┐
                                  ▼
        ┌─────────────────► run round N ──────────────┐
        │                       │                      │
        │                  error? ──yes──► recover (below) ──► retry / next
        │                       │ no
        │                  read real result, log it
        │                       │
        │                  STOP condition met? ──yes──► stop + report
        └──────────────────────┘ no, plan next change
```

## Iteration budget

- **Parse a cap from the initial prompt** — "up to 8 rounds", "max 3 tries", "10 experiments"
  → that integer is the hard cap.
- **No cap given → default to 5 rounds.** Announce the default so the user can override.
- Track and announce the round ("Round 3/5"). Never silently exceed the cap.

## Error recovery — loop through failures, don't die on them

When a run errors, the loop's job is to recover, not to stop at the first traceback. But
retries are bounded — never an infinite loop.

1. **Read the actual error** — full traceback/stderr. Quote it exactly; the last frame
   usually names the file + line. Don't guess from the symptom.
2. **Classify it:**
   - **Recoverable — fix and retry:** code bug, shape/dtype mismatch, NaNs/infs, bad
     hyperparameter, OOM (reduce batch size / subsample / smaller model), non-convergence
     (lower LR, more iters), a transient timeout.
   - **Needs a different approach:** the model family / feature set / config is fundamentally
     failing — change strategy next round rather than retrying the same thing.
   - **Blocked — stop and ask:** missing/inaccessible data, a dependency you shouldn't install
     unprompted, a missing credential, an infeasible target. State exactly what's needed.
3. **Fix at the cause, retry once changed.** One fix per retry so you know what worked
   (pairs with `debug-rca`).
4. **Bounded retries** — cap retries on the *same* error (e.g. 2–3). Same error after the cap
   = treat as blocked or change approach; do not loop forever on an unfixable failure.
5. **A retry consumes effort, not necessarily a round** — distinguish "fixing this round's run"
   from "starting a new experiment round". Recovering a crash isn't a wasted iteration.

## Track progress every round

Keep a running log so the loop is auditable and the stop decision is grounded:
round number, what changed, config (+ seed), result vs the target, and outcome
(improved / no change / errored). The "best so far" is always known.

## Stop conditions — halt when ANY is true

1. **Done** — the definition of done is met and verified (not just "ran without error").
2. **Told to stop** — the user said stop / cancel / that's enough. Halt immediately, report
   the best result so far. The user's instruction overrides remaining budget.
3. **Blocked** — needs something only the user can provide (see recovery step 2). State it.
4. **Cap reached** — iteration budget exhausted. Report the best result.
5. **Plateau** — a round (or two) yields no meaningful improvement and there's no materially
   different idea left. Stop early rather than burning budget on noise.

## On stopping, always report

What was achieved vs the target; what was tried each round and why the best won; whether
"done" was met; if not, the gap + most promising next step; and how to reproduce.

## Guardrails

- **Never loop forever.** Every loop has a cap and bounded per-error retries. Unbounded
  retry on an unfixable error is the failure mode this skill exists to prevent.
- **"Ran" ≠ "done."** A round that executes without error but doesn't meet the bar is not
  success — judge against the definition of done (pairs with `verify-before-done`).
- **Hitting the cap is not a goal** — if done in round 2, stop at round 2.
- **Report honestly** — real results each round, errors included; never claim progress a log
  entry doesn't support.
- **A user stop overrides everything** — never keep running after being told to stop.
