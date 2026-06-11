---
name: problem-framing
description: >
  Turn a prompt or problem statement into a concrete ML spec before any code: task type,
  target, evaluation metric, data, constraints, and an explicit definition of done. Built to
  later accept a richer source (e.g. a Jira ticket). Use at the start of any ML task, or when
  the user says "frame this", "what kind of ML problem is this", "define the task / metric /
  success criteria".
---

# Problem Framing

Most ML failures are framing failures — the wrong metric, an ambiguous target, no clear bar
for "done". Spend the first move turning the request into a spec everything downstream is
judged against. No data loading or modeling until the spec is written.

## Produce this spec

1. **Task type** — classification (binary / multiclass / multilabel), regression, ranking,
   forecasting, clustering, recommendation, etc. State it explicitly.
2. **Target / label** — what is being predicted, in what units/classes. Confirm it exists in
   the data (or flag that labels must be created/sourced).
3. **Evaluation metric** — the single primary metric to optimize (e.g. ROC-AUC, F1, RMSE,
   MAE, MAP@k), plus any secondary/guardrail metrics. Match the metric to the real objective
   and to class balance — accuracy on a 99/1 split is the classic trap.
4. **Definition of done** — the concrete bar: a metric target ("AUC ≥ 0.85"), "beat this
   baseline", or "best achievable within the iteration budget". This is the loop's stop test.
5. **Data** — where it is, its shape/size, the rough feature set, and how train/val/test will
   be split (random, stratified, temporal). Note leakage risks up front.
6. **Constraints** — latency/size limits for deployment, interpretability needs, compute
   budget, time/iteration budget, fairness or regulatory constraints.

## Choosing the metric (don't skip this)

- Imbalanced classes → ROC-AUC / PR-AUC / F1, not accuracy.
- Costly false negatives vs false positives → weight precision/recall accordingly.
- Regression with outliers → MAE or Huber over RMSE if large errors shouldn't dominate.
- Ranking/recommendation → MAP@k, NDCG, not pointwise error.
- Match the metric to the decision the model drives, not to what's easy to compute.

## Resolve ambiguity before modeling

- If the target, metric, or done-bar is genuinely undefined and you can't infer a sensible
  default, state the assumption you're making — or, if it's load-bearing, stop and ask. A
  model optimized for the wrong metric is wasted compute.

## Input sources

The input today is a prompt or written problem statement. This step is the integration point
for a richer source later (e.g. a Jira ticket): treat the ticket's summary + description as
the problem statement and its acceptance criteria as the definition of done.

## Guardrails

- No data loading or modeling until task type, metric, and definition of done are fixed.
- Pick the metric for the real objective, not convenience; call out the class-balance trap.
- Write the split strategy now — deciding it after seeing results invites leakage.
- Record the spec so every later round is judged against the same bar.
