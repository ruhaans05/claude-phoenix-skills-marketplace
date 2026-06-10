# safe-refactor

Improves code structure **without changing behavior**, in small reversible steps under a test
safety net — re-running the net after every step.

**Why it helps:** the danger in refactoring is silently breaking something while "improving"
it. Safety comes from green tests plus small reversible increments, not from being careful in
one big leap.

**What it does:**
- Establishes (or adds) a green test net before touching anything.
- Refactors in commit-sized, behavior-preserving steps; reverts any step that goes red.
- Reuses existing utilities instead of adding another copy; matches the surrounding style.

**Guardrail:** never mixes a behavior change or bug fix into a refactor (separate, labeled
step), never restructures untested code without first pinning its behavior.

Trigger phrases: "refactor this", "clean this up", "extract a function", "simplify", "DRY
this".
