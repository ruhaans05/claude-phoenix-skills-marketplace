---
name: model-sweep
description: >
  Train a baseline plus a spread of candidate algorithms quickly and rank them on the
  validation metric, to find which model families are worth tuning. Cheap-and-fast first,
  with sensible defaults. Use after data is prepped, or when the user says "try different
  models", "which algorithm", "run a model sweep", "establish a baseline".
---

# Model Sweep

Before spending compute tuning, find out which model families even fit the problem. Train a
baseline and a broad-but-fast set of candidates on the shared split, rank them on the
validation metric, and pass the top few to tuning. Breadth first, depth later.

## Always start with a baseline

A baseline makes every later number meaningful — "AUC 0.82" means nothing without it.

- Classification → majority-class / stratified random, or logistic regression.
- Regression → mean/median predictor, or linear regression.
- Forecasting → naive (last value / seasonal naive).

Any candidate that can't beat the baseline is a red flag (bug, leakage, or wrong framing).

## Sweep candidates (match to data type)

- **Tabular** — logistic/linear regression, random forest, gradient boosting
  (XGBoost / LightGBM / CatBoost — usually the tabular front-runners), SVM, k-NN.
- **Text** — TF-IDF + linear as a strong baseline; transformer fine-tune if warranted.
- **Images** — a small CNN or a pretrained backbone (transfer learning) over training
  from scratch.
- **Time series** — naive/seasonal baselines, then classical (ARIMA/ETS) and/or gradient
  boosting on lag features.

## How to sweep

1. Train each candidate with **sensible defaults** (no tuning yet) on train, score on val
   with the primary metric — same split for all, so the comparison is fair.
2. Keep each candidate cheap: cap training time, subsample if the data is large, use early
   stopping where available.
3. **Rank on the validation metric**; note training time and stability alongside the score.
4. Sanity-check: candidates clustered near the baseline → revisit features/framing before
   tuning. One candidate near-perfect → suspect leakage, don't celebrate.
5. Pass the **top 1–3** families to `train-tune`. Prefer a model that also fits the
   constraints from framing (latency, size, interpretability).

## Guardrails

- Always include a baseline; report every candidate against it.
- Identical split and metric for every candidate — no moving goalposts.
- Validation only here — the test set stays untouched until final selection.
- Don't over-invest in the sweep: defaults are enough to rank; tuning comes next.
- A suspiciously dominant candidate is a leakage signal, not a win — investigate first.
