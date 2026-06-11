# train-tune

Spends compute on the sweep's front-runners — hyperparameter search, regularization, early
stopping — all judged on validation, with the test set kept sealed.

**Why it helps:** tuning pushes the promising models to their potential, but it's where
overfitting-to-validation and test-set leakage creep in. This skill makes the search
deliberate, budget-aware, and leakage-safe.

**What it does:**
- Tunes the top 1–3 candidates (depth on the few, not shallow tuning of all).
- Picks grid / random / Bayesian search by budget; validates via CV or the fixed split.
- Uses early stopping and regularization; logs every trial's params + score for reproducibility.

**Guardrail:** hyperparameters chosen on validation/CV only (never the test set); watches for
overfitting the validation set; keeps the winning config within framing constraints.

Trigger phrases: "tune the model", "hyperparameter search", "optimize the model", "improve
the score".
