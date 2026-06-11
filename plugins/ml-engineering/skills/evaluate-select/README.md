# evaluate-select

Evaluates tuned candidates on held-out test data, runs leakage / overfit / calibration /
per-segment checks, selects the winner, and reports honestly against the definition of done.

**Why it helps:** a validation score isn't proof. This is the one and only test-set
evaluation, plus the trust checks that catch a fake-good result before it's declared a win.

**What it does:**
- Scores the finalist on the untouched test set vs baseline and the done-bar.
- Runs overfit, leakage, per-segment, calibration, and stability checks.
- Selects within framing constraints (latency/size/interpretability), not just top metric,
  and reports with full reproduction details.

**Guardrail:** the test set is evaluated exactly once, for the finalist; real numbers only;
a too-good score is assumed to be leakage until proven otherwise.

Trigger phrases: "evaluate the model", "is it good enough", "pick the best model", "final
evaluation".
