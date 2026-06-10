# verify-before-done

Stops the agent from claiming a task is done, fixed, or working without actually running it
and observing the result — then reports failures and skips honestly.

**Why it helps:** "it should work" is not "it works". The gap between reading code and running
it is where false completion claims live, and an unverified "done" erodes trust faster than
an honest "not yet".

**What it does:**
- Runs the real thing (tests/build/app/command) and reads the actual output.
- Matches the proof to the claim — reproduces the original bug to confirm a fix, exercises a
  new feature.
- Reports plainly: verified done (with what was run), failures (with output), or skipped (and
  why).

**Guardrail:** never marks something complete to end the turn faster, never adjusts a test to
make a failure look like a pass, distinguishes "verified" from "should work".

Trigger phrases: "did it work", "are you sure", "verify this".
