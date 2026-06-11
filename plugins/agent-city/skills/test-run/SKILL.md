---
name: test-run
description: >
  The city-inspector's verdict phase: run the full relevant suite — new tests and
  pre-existing ones — read the real output, and triage every failure to a root cause the
  city-engineer can act on. Produces the inspection report that gates the courier. Use
  after test-forge, after every repair pass, and whenever "does it actually pass?" needs
  a real answer.
---

# Test Run

Run everything relevant, read what actually happened, and turn every red into a diagnosis.
The pipeline moves on real results only.

## Run

- The **full relevant suite**: the forged tests AND the pre-existing ones. A change that
  breaks old behavior is a failed inspection even if its own tests pass.
- Integration tests run against the real provisioned database or a faithful test
  instance — not mocks pretending to be the seam they're meant to test.
- Capture the real output. Counts and failures come from it, never from memory or
  expectation.

## Triage every failure

For each red, read the actual failure output and classify:

| Root cause | Action |
|------------|--------|
| **Code bug** | Diagnosis to the engineer: failing test, expected vs. actual, suspected cause with `file:line`, smallest reproduction |
| **Bad test** (wrong assertion, brittle fixture, hidden order dependency) | Fix the test yourself; record what was wrong and why the fix is legitimate |
| **Environment** (missing dep, service not running, config) | Fix if mechanical; otherwise report the precise blocker to the Mayor |

"Bad test" requires proof the *code* is right — tracing the behavior and showing the
assertion wrong. The default presumption is the test caught something.

## The inspection report

```markdown
## Inspection Report
- **Command:** <exact test command(s) run>
- **Result:** <passed>/<failed>/<skipped> (real numbers from output)
- **Verdict:** PASS → courier | FAIL → engineer
- **Failures:** <per failure: test, root cause class, diagnosis, file:line>
- **Skips:** <each skip and its stated justification — none silent>
```

## Guardrails

- Never delete, skip, or weaken a test to go green. A red exits inspection only by the
  code being fixed or the test being proven wrong.
- Never report counts you didn't observe. Suite couldn't run at all → that *is* the
  report, marked FAIL with the blocker named.
- Flaky test (passes on retry without changes) → run it again; if instability is
  confirmed, report it as a finding with its failure mode — don't average it away.
- PASS verdict requires zero unexplained failures and zero silent skips.
