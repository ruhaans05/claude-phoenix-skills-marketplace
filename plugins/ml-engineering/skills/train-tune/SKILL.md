---
name: train-tune
description: >
  Spend compute on the top model candidates: hyperparameter search, regularization, early
  stopping, all judged on validation — never the test set. Use after a sweep picks front-
  runners, or when the user says "tune the model", "hyperparameter search", "optimize the
  model", "improve the score".
---

# Train & Tune

Tuning is where the sweep's front-runners get pushed to their potential — but it's also where
overfitting to the validation set and test-set leakage creep in. Search deliberately, judge
on validation, and keep the test set sealed.

## Approach

1. **Tune the top 1–3 candidates** from the sweep, not everything — depth on the promising
   few beats shallow tuning of all.
2. **Pick a search strategy by budget:**
   - Small space / quick → grid search.
   - Larger space → random search (often beats grid per unit compute).
   - Expensive models, bigger budget → Bayesian / Optuna-style.
3. **Validate properly** — cross-validation or the fixed validation split. CV gives a more
   stable estimate but costs more; match to budget.
4. **Tune the parameters that matter** per family — e.g. gradient boosting: learning rate +
   n_estimators (with early stopping), tree depth, subsample/colsample, regularization.
   Don't grind on parameters with little effect.
5. **Use early stopping** on a validation fold for iterative learners — caps compute and
   limits overfitting.
6. **Regularize** to close a train/val gap: L1/L2, dropout, tree constraints, more data /
   augmentation, or fewer features.

## Reproducibility & efficiency

- Fix and record random seeds; log every trial's params + validation score so the search is
  auditable and the best config is reproducible.
- Cap time/trials per candidate; start coarse, then refine around promising regions.
- Reuse the same prepared split and metric from earlier steps — comparability depends on it.

## Guardrails

- **Tune on validation / CV only. The test set is never used to choose hyperparameters** —
  that's leakage and inflates the reported score.
- Watch for overfitting the validation set itself (many trials → optimistic val score). A
  modest, robust config often generalizes better than the absolute val-best.
- Stop tuning when gains flatten — diminishing returns are the orchestrator's plateau signal.
- Keep the best config within the framing constraints (latency, model size, interpretability),
  not just the top metric.
- Record real trial results; never report an untrained or assumed score.
