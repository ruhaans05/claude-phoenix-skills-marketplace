# experiment-loop

The autonomous iteration engine: keep running tasks — training, tuning, fixing errors — round
after round, recovering from failures, tracking progress, and deciding continue vs stop, until
done / blocked / a plateau / an iteration cap / told to stop.

**Why it helps:** turning a one-shot task into a reliable "keep going until done" loop is its
own discipline — bounded retries, real error recovery, progress tracking, and clear stop
conditions. Without it, an agent either gives up at the first traceback or loops forever.

**What it does:**
- Parses an iteration cap from the prompt (default 5 rounds), announces and tracks rounds.
- Recovers from errors: reads the real traceback, classifies recoverable vs needs-new-approach
  vs blocked, fixes at the cause, retries with a bounded cap — never infinitely.
- Logs every round (config, seed, result vs target) and stops on done / told-to-stop / blocked
  / cap / plateau, then reports the best result and how to reproduce.

**Guardrail:** never loops forever (capped rounds + bounded per-error retries); "ran" ≠ "done";
a user stop overrides remaining budget; reports only results the log supports.

The `ml-engineer` agent runs on this loop; the discipline is domain-agnostic and reusable.

Trigger phrases: "loop until done", "keep training until", "iterate until it works", "run
experiments until", "auto-retry on errors", "max N iterations".
