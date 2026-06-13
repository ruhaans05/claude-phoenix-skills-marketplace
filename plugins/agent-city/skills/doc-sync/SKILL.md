---
name: doc-sync
description: >
  The city-herald's phase: after tests pass and before the PR opens, bring the
  human-facing docs back into truth with what the run actually built. Update the README's
  feature list and usage examples, refresh any command/flag/API surface that changed, add
  a CHANGELOG entry, and touch the docs the change affected — adding what's new, correcting
  what's now wrong, removing what's gone. Documentation ships in the same PR as the code.
  Use after test-run is green, or on demand to audit docs against the code.
---

# doc-sync

Runs once per pass, between a green `test-run` and `pr-open`. The job: leave the docs
saying exactly what the code now does — no more, no less — so the PR a reviewer opens
has code and documentation that agree.

## Inputs

- The run's diff (what construct changed) and the inspector's real test results.
- The existing docs: `README.md` first, then `CHANGELOG.md`, help/usage text, and any
  `docs/` pages or doc comments the change touched.

## What to sync

| Surface | When it needs an edit |
|---------|------------------------|
| **README — features/usage** | A new capability, a changed command/flag/endpoint, a new install or config step, a removed feature |
| **CHANGELOG** | Every user-visible change — one entry under the right version/heading, in the project's existing format |
| **CLI/`--help` / API reference** | The invocation surface changed: new subcommand, renamed flag, altered output, new/changed route |
| **Inline doc comments / docstrings** | A public function or type's contract changed enough that its own docs now lie |
| **`docs/` pages, examples** | The project keeps longer-form docs and the change reaches them |

## How

1. **Diff against the docs.** Walk the change set; for each user-visible delta, find where
   the docs describe it (or should). Build a short list: add / correct / remove.
2. **Edit to the house voice.** Match the existing structure, tone, and formatting
   exactly. Examples must be runnable and traceable to code that exists.
3. **Write the CHANGELOG entry** from the inspector's real numbers and the actual change —
   never from the work order's wishlist.
4. **Stop at the smallest honest edit.** Touch only what the change affects; leave
   untouched sections alone.
5. **Log it** in the ledger: which docs were updated, or "no doc change needed" with the
   one-line reason.

## Guardrails

- **Truth over completeness.** Better a short README that's correct than a thorough one
  that claims a feature the tests never exercised. Every line traces to shipped, verified
  code — the marshal's data-quality deputy checks exactly this at the doc-sync checkpoint.
- **Docs only.** Markdown, help text, doc comments, examples. Never edit code or tests to
  make a doc true; if an example can't be honest without a code change, report it.
- **No secrets, no internals, no ledger copy.** Public docs stay public-safe — env var
  names and placeholders only.
- **A no-op is a valid result.** A pure internal refactor may need zero doc edits. Record
  the judgment and pass; don't manufacture churn.
