# debug-rca

Debugs by root-cause analysis instead of guess-patching: reproduce → isolate → find the true
cause → fix the cause → verify (plus a regression test).

**Why it helps:** guess-patching trades one bug for two. Fixing the symptom (a null check at
the crash site) leaves the real defect upstream. RCA finds the actual cause so the fix sticks
and doesn't regress.

**What it does:**
- Gets a reliable minimal reproduction and captures the exact error.
- Isolates the failing path by bisecting and testing one hypothesis at a time.
- Fixes at the cause, then verifies the repro passes and nothing else broke.

**Guardrail:** never ships a fix it can't explain, never silences the symptom, reports
honestly when the cause is still unknown.

Trigger phrases: "fix this bug", "why is this failing", "debug this", "it crashes when".
