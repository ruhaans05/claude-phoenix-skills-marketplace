# context-prune

Trims stale, dead, and superseded content out of the working context so the rest of the run
costs fewer input tokens.

**Why it saves money:** input tokens are re-paid on every single turn. A big context built
up from early exploration keeps charging you for the whole rest of the run. Pruning it once
stops the cost from compounding.

**What it does:**
- Flags the signs of a bloated context (old tool dumps, re-read files, dead-end searches).
- Says clearly what's safe to drop vs what must be kept.
- Replaces bulky raw output with its one-line conclusion — keep the finding, drop the dump.

**Guardrail:** never prunes context you're actively reasoning over; correctness beats
savings.

Trigger phrases: "prune context", "trim the context", "too many tokens", "drop stale
output", "compact".
