---
name: debug-rca
description: >
  Debug by root-cause analysis instead of guess-patching: reproduce, isolate, find the true
  cause, fix it, verify the fix and that nothing else broke. Use when something is broken,
  output is wrong, a test fails, or the user says "fix this bug", "why is this failing",
  "debug this", "it crashes when".
---

# Debug — Root Cause Analysis

Guess-patching trades one bug for two. RCA finds the actual cause so the fix is real and
doesn't regress. Discipline beats cleverness here.

## The loop

1. **Reproduce.** Get a reliable, minimal repro — the exact command/input that triggers it
   and the exact observed vs expected. If you can't reproduce it, you can't confirm a fix.
2. **Capture the evidence.** Read the full error/stack/log. Quote the error exactly; don't
   paraphrase. The first failing frame usually names the file and line.
3. **Isolate.** Narrow to the smallest code path that still fails. Bisect: comment out,
   add a targeted log/assert, check the boundary where good input becomes bad output. Form
   one hypothesis at a time and test it.
4. **Find the root cause.** Trace *why* the bad state arises, not just where it surfaces. The
   crash site is often downstream of the real defect. Keep asking "why" until the cause
   explains every symptom.
5. **Fix at the cause**, not the symptom. A null check at the crash hides a wrong value set
   three functions earlier — fix the source.
6. **Verify.** Re-run the repro: now passes. Run the surrounding tests: nothing else broke.
   Add a **regression test** that fails without the fix (hand to `tdd-loop`).

## Tactics

- Change one thing at a time; re-run between changes so you know what moved the needle.
- Trust evidence over assumptions — confirm the value is what you think with a log/debugger.
- For dynamic dispatch (callbacks, events, async), trace the actual call path, don't assume.
- Recent regression? Check what changed (`git log`/`git blame` around the failing line).

## Guardrails

- Don't ship a fix you can't explain. "It works now" without a known cause means it'll
  return.
- Remove debug logs/scratch instrumentation before finishing (or mark them clearly).
- Report honestly: if root cause is still unknown, say so and state the leading hypothesis.
- Never silence the symptom (swallow the error, loosen the assert) to make it "pass".
