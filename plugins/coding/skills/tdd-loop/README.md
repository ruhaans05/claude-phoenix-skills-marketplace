# tdd-loop

Drives a feature with a test-first loop: failing test → minimum code to pass → run the suite
→ iterate to green → refactor. Every step is verified by running the real tests.

**Why it helps:** test-first turns "looks done" into "proven done". The agent loops against
actual test output instead of guessing, which is the highest-confidence way to add behavior
in an agentic run.

**What it does:**
- Runs the red → green → refactor cycle in small, one-behavior increments.
- Mirrors the project's existing test framework, location, and fixture style.
- Always runs the full relevant suite before declaring done.

**Guardrail:** never edits a test just to pass, never skips/deletes a failing test to go
green, never claims green without running it.

Trigger phrases: "write tests", "TDD this", "add X with tests", "make it pass".
