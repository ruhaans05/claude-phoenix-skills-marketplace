---
name: data-prep
description: >
  Load, inspect, clean, split, and feature-engineer data for an ML task — leakage-safe from
  the start. Split before fitting any transform; build a reusable train/val/test that every
  experiment round shares. Use after the task is framed, or when the user says "prep the
  data", "clean this dataset", "feature engineering", "split the data".
---

# Data Prep

The dataset decides the ceiling; modeling only approaches it. Get the data right and
leakage-free before any model runs, and produce a fixed split that every round of the loop
reuses — so round-to-round comparisons are valid.

## Steps

1. **Load & inspect.** Shape, dtypes, head, summary stats, target distribution. Quantify
   missingness per column and class balance. Look before transforming.
2. **Split first.** Create train/val/test (or CV folds) **before** fitting any transform.
   Stratify for classification; split by time for temporal data; group-split when rows share
   an entity to avoid the same entity in train and test. Fix the seed and reuse this split
   for the whole run.
3. **Clean** — on train, applied to val/test via fitted transforms: handle missing values,
   fix dtypes, deduplicate, treat outliers, normalize categories.
4. **Feature engineering** — encode categoricals, scale where the model needs it, build
   domain features, handle dates/text. Fit every transform on **train only**, then apply to
   val/test.
5. **Validate the prepared data** — no NaNs/infs where the model can't take them, shapes
   align, no obvious leak (a feature suspiciously correlated with the target → investigate).

## Leakage — the cardinal sin (check every item)

- **Fitting before splitting** — a scaler/encoder/imputer fit on the full dataset leaks
  test statistics into train. Always split first, fit on train only.
- **Target leakage** — features computed using the target, or available only after the
  prediction moment (a "was_refunded" feature predicting "will_churn").
- **Temporal leakage** — using future information to predict the past; random-splitting time
  series. Split by time.
- **Group leakage** — the same user/patient/device in both train and test inflates the score.
- **Duplicate rows** spanning the split.

## Guardrails

- Split before any fit. This single rule prevents most leakage.
- Pipe transforms (e.g. sklearn `Pipeline`/`ColumnTransformer`) so fit-on-train /
  apply-on-test is enforced structurally, not by hand.
- Never impute, scale, or select features using val/test statistics.
- A feature that makes the score near-perfect is a leak until proven legitimate — investigate
  before celebrating (pairs with `evaluate-select` and `verify-before-done`).
- Persist the split + seed so every model trains and is judged on identical data.
