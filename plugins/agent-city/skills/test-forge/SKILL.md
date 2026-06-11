---
name: test-forge
description: >
  The city-inspector's test-writing phase: forge unit tests for each new behavior and
  integration tests for the seams (API ↔ logic ↔ database), mirroring the project's
  existing framework and conventions. Use after construct completes and before test-run,
  or whenever code lacks the tests its behavior deserves.
---

# Test Forge

The engineer's report says what the code should do; the work order's definition of done
says what the user needs it to do. Forge the tests that check both — tests that would
fail if the code were wrong.

## What to forge

- **Unit tests** — one per behavior the change introduces: the happy path, then the edges
  (empty, boundary, malformed input, error path). Small, fast, isolated.
- **Integration tests** — the seams: route → handler → logic, logic → database (the real
  provisioned one or a faithful test instance), service → external API (faked at the
  boundary, never called live in tests).
- **The definition of done, as a test.** The work order's acceptance behavior gets at
  least one end-to-end check that proves it. This is the pipeline's exit criterion made
  executable.

## How to forge

- **Mirror the project.** Same framework, same directory layout, same fixture and naming
  style as existing tests. No existing tests → pick the stack's standard (pytest, Jest,
  go test, JUnit…), set it up minimally, and note the choice for the PR description.
- **Assert behavior, not implementation.** Test what the caller observes — return values,
  state changes, emitted effects — not private internals. Refactors shouldn't break the
  suite; bugs should.
- **Watch each new test fail first** (against pre-change code, or by reverting the
  assertion) — a test that can't fail verifies nothing. Confirm it fails for the right
  reason, not a typo or import error.
- **Deterministic only.** No live network, no wall-clock time, no unseeded randomness.
  Pin or fake them at the boundary.

## Guardrails

- Don't forge filler. Each test pins a behavior that matters to the work order; coverage
  theater wastes the iteration budget.
- If the code resists testing (hardwired dependencies, no seams), that's a finding —
  report it to the engineer as a design defect rather than writing a contorted test
  around it.
- Tests are honest observers: no asserting around a known bug to keep the pipeline
  moving. Found mid-forge → it goes in the triage report.
