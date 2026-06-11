# data-prep

Loads, inspects, cleans, splits, and feature-engineers data for an ML task — leakage-safe
from the start — and produces a fixed train/val/test that every experiment round reuses.

**Why it helps:** the dataset sets the performance ceiling; modeling only approaches it. And
the single biggest way to get a fake-good result is data leakage. This skill makes the split
and the fit-on-train-only discipline structural.

**What it does:**
- Inspects shape, missingness, and class balance before transforming.
- Splits before fitting any transform (stratified / temporal / group-aware), fixed seed.
- Engineers features with train-only fits applied to val/test, then validates the result.

**Guardrail:** split-before-fit always; actively hunts target/temporal/group leakage; treats
a near-perfect feature as a leak until proven legitimate.

Trigger phrases: "prep the data", "clean this dataset", "feature engineering", "split the
data".
