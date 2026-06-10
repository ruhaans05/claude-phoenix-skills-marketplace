---
name: verify-before-done
description: >
  Never claim a task is done, fixed, or working without actually running it and observing the
  result. Run the test/build/app, read the real output, and report failures and skips
  honestly. Use before saying "done"/"fixed"/"works", at the end of any change, or when the
  user says "did it work", "are you sure", "verify this".
---

# Verify Before Done

"It should work" is not "it works". The gap between reading code and running it is where
false completion claims live — and an unverified "done" erodes trust faster than an honest
"not yet". Close the loop before you declare it closed.

## Before saying done / fixed / works

1. **Run the real thing.** The test suite, the build, the linter, the actual app or command —
   whatever proves the change does what it should. Reading the diff is not verification.
2. **Observe the actual output.** Read it. Don't infer success from "it ran" — check the
   result is what you expected (test passed, page rendered, value correct, exit code 0).
3. **Verify the specific claim.** If you fixed a bug, reproduce the original failure and
   confirm it's gone. If you added a feature, exercise it. Match the proof to the claim.
4. **Check for collateral damage.** The change you made didn't break the tests around it.
5. **Report faithfully:**
   - Done and verified → state it plainly, with what you ran. No hedging.
   - Tests failed → say so, with the actual output. Don't bury it.
   - A step was skipped or couldn't be run → say that explicitly and why.

## Honesty rules

- Don't claim a test passes you didn't run. Don't claim a feature works you didn't exercise.
- Distinguish "verified" from "should work" in your wording — they are different states and
  the user needs to know which one you mean.
- If you can't verify (no way to run it here, missing access), say so and state exactly what
  the user needs to check, rather than implying it's confirmed.
- Partial completion is a real status — report what's done, what's verified, and what remains.

## Guardrails

- Never mark something complete to end the turn faster. An unverified claim that fails costs
  far more than the time saved.
- Never adjust the test/expectation to make a real failure look like a pass.
- "No errors in the output I read" is only verification if you actually read the output.
