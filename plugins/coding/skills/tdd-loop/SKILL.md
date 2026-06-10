---
name: tdd-loop
description: >
  Drive a feature with a test-first agentic loop: write a failing test, implement the minimum
  to pass, run the suite, iterate to green, then refactor. Each step is verified by running
  the real tests. Use when adding a feature/function with checkable behavior, or when the
  user says "write tests", "TDD this", "add X with tests", "make it pass".
---

# TDD Loop

Test-first turns "looks done" into "proven done". The test is the spec and the verifier; the
agent loops against real test output instead of guessing. This is the highest-confidence way
to add behavior in an agentic run.

## The loop (red → green → refactor)

1. **Red — write a failing test** that pins the desired behavior. Run it; confirm it fails
   for the *right reason* (asserts the behavior, not a typo/import error).
2. **Green — minimum implementation** to make that test pass. Don't gold-plate.
3. **Run the suite** — the new test AND the existing ones. Read the real output.
4. **Iterate** — if red, fix and re-run. Loop until green. Don't move on while red.
5. **Refactor** — clean up names/structure with the green suite as a safety net; re-run to
   confirm still green. (Pairs with `safe-refactor`.)

## Before the loop

- Find how tests are run and where they live (`explore-first`): the test command, framework,
  fixture style. Mirror existing tests — same structure, same helpers.
- Scope each cycle to one small behavior. Many small red→green cycles beat one giant test.

## What makes a good test here

- Asserts **behavior/output**, not implementation details (so refactor doesn't break it).
- Fails first — an always-green test verifies nothing.
- Covers the obvious case, then an edge case (empty, boundary, error path).
- Runs fast and deterministically — no real network/time/random unless pinned.

## Guardrails

- Never edit the test just to make a real failure pass — that hides the bug. Fix the code.
- Run the **full** relevant suite before declaring done, not just the new test.
- If tests fail, report it with the actual output. Don't claim green unverified.
- Don't delete or `skip`/`xfail` a failing test to go green. If a test must be skipped,
  state why and the condition to re-enable it.
