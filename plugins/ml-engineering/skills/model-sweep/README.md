# model-sweep

Trains a baseline plus a spread of candidate algorithms fast and ranks them on the validation
metric — so you know which model families are worth tuning before spending compute.

**Why it helps:** tuning the wrong model family is wasted compute. A quick breadth-first sweep
with sensible defaults shows what fits the problem, and a baseline makes every later number
meaningful.

**What it does:**
- Always establishes a baseline first (majority-class / mean / naive) to anchor results.
- Sweeps candidates matched to the data type (gradient boosting, linear, trees, transfer
  learning, classical TS, …) on the shared split with defaults.
- Ranks on the validation metric and passes the top 1–3 families to tuning.

**Guardrail:** identical split + metric for every candidate; validation only (test untouched);
a near-perfect candidate is treated as a leakage signal, not a win.

Trigger phrases: "try different models", "which algorithm", "run a model sweep", "establish a
baseline".
