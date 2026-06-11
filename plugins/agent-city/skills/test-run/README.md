# test-run

The city-inspector's verdict phase: runs the full relevant suite (new tests + pre-existing
ones), reads the real output, and triages every failure into a root-cause diagnosis the
engineer can act on. Its inspection report is the gate the courier waits behind.

**Why it helps:** the pipeline loops on failures instead of dying on them — but only if
each red comes back as a *diagnosis* ("session token issued before password check,
auth.py:42") instead of a bare "test failed".

**What it does:**
- Runs everything relevant; integration tests hit the real provisioned database.
- Classifies each failure: code bug → engineer, bad test → fixed with proof, environment
  → fixed or escalated.
- Reports real pass/fail/skip numbers from actual output, with a PASS/FAIL verdict.

**Guardrail:** never deletes/skips/weakens a test to go green, never reports unobserved
counts, never averages a flaky test away. "Bad test" requires proving the code right.

Trigger phrases: runs after `test-forge` and after every repair pass; also "run the
tests", "does it pass?".
