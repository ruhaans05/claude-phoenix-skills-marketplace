---
name: evaluate-select
description: >
  Evaluate tuned candidates on held-out test data, run leakage / overfit / calibration
  checks, select the winner, and report honestly against the definition of done. The one and
  only test-set evaluation. Use after tuning, or when the user says "evaluate the model",
  "is it good enough", "pick the best model", "final evaluation".
---

# Evaluate & Select

This is the moment of truth and the **only** time the test set is touched. Score the tuned
candidate(s) on held-out data, prove the result is real (not leakage or overfit), select the
winner, and report it straight against the definition of done.

## Evaluate on held-out test data

1. Score the selected model on the **untouched** test set with the primary metric, plus
   relevant secondary metrics:
   - Classification → precision, recall, F1, ROC-AUC / PR-AUC, confusion matrix; calibration
     if probabilities are used downstream.
   - Regression → RMSE, MAE, R², residual plots.
   - Ranking/recommendation → MAP@k, NDCG, coverage.
2. Compare to the **baseline** and to the **definition of done** from framing. State plainly
   whether the bar is met.

## Trust checks — is the score real?

- **Overfitting** — large train-vs-test gap → the model memorized; prefer a simpler/more
  regularized candidate.
- **Leakage (again)** — a too-good test score, or a feature dominating importance
  suspiciously → re-audit features and the split before believing it. A great score is a
  bug until proven otherwise.
- **Per-segment performance** — check error across important slices/classes, not just the
  aggregate; a strong average can hide a failing subgroup.
- **Calibration** — if probabilities drive decisions, check reliability, not just ranking.
- **Stability** — sensible across CV folds / seeds, not a lucky split.

## Select & report

- Pick the model that best meets the metric **within the framing constraints** (latency, size,
  interpretability) — not blindly the top score.
- Report: chosen model + key hyperparameters; test metric vs baseline vs target; what was
  tried and why this won; known weaknesses; and exact reproduction (data split, seed, command).
- State whether the definition of done is met. If not, give the gap and the most promising
  next step — this feeds the orchestrator's continue/stop decision.

## Guardrails

- **Evaluate on the test set exactly once, for the finalist.** Repeatedly checking test while
  iterating turns it into a validation set and inflates the result — that's leakage.
- Report the real number — never round up, cherry-pick a lucky run, or present a validation
  score as a test score.
- Selecting the highest metric while ignoring overfitting, a failing subgroup, or a blown
  latency budget is a false win — weigh the trust checks and constraints.
- If results look too good, assume leakage and investigate before reporting success (pairs
  with `verify-before-done`).
