# doc-sync

Keeps docs true to the code after a change — finds documentation the change made stale
(docstrings, README, API docs, comments, examples) and updates it to match the new behavior.

**Why it helps:** stale docs are a silent bug — the code changed, the doc didn't, and now the
doc lies. The most reliable moment to keep docs honest is right after the code changes.

**What it does:**
- Diff-driven: for each changed signature/return/flag/endpoint/default, finds every place it's
  documented.
- Compares each doc against the new reality and updates the stale ones.
- Flags whether the change warrants a CHANGELOG entry.

**Guardrail:** treats doc-sync as part of the change (not a later chore), verifies updated
examples still run, stays in scope (no wholesale rewrite), and surfaces ambiguous cases
instead of guessing.

Trigger phrases: "update the docs", "are the docs still right", "the docs are stale", "sync
docs".
