# doc-sync

The city-herald's phase: after the tests pass and before the PR opens, bring the docs
back into truth with what the run actually built — and ship those doc edits in the same
PR as the code.

**Why it helps:** documentation rots the moment code moves without it, and stale docs
mislead worse than missing ones. Making "the README is current" a pipeline phase — placed
*after* the green test run, so it describes what was verified, not what was hoped — means
the project's docs are never behind the code and never ahead of it. A reviewer never opens
a PR where the README and the implementation disagree.

**What it touches:**
- README — feature list, usage examples, install/config steps
- CHANGELOG — one entry per user-visible change, in the project's existing format
- CLI `--help` / API reference — when the invocation surface changed
- inline doc comments and `docs/` pages the change reached

**Guardrail:** docs only — never edits code or tests to make a doc true. Every line
traces to shipped, verified code (the marshal's data-quality deputy checks this at the
doc-sync checkpoint). No secrets, no internal paths, no copy of the ledger. And a pure
internal refactor that needs no doc change is allowed to be a no-op — the herald records
that and passes rather than manufacturing churn.

Trigger phrases: runs automatically after a green test run; also "update the README",
"the docs are out of date", "document this change", "sync the changelog".
