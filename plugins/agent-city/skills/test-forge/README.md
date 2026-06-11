# test-forge

The city-inspector's test-writing phase: forges unit tests for each new behavior,
integration tests for the seams, and at least one end-to-end test that makes the work
order's definition of done executable.

**Why it helps:** the pipeline's exit criterion is a passing PR — which only means
something if the tests would actually fail on wrong code. Every forged test is watched
failing first, so green is evidence, not decoration.

**What it does:**
- Unit tests: happy path + edges for each new behavior.
- Integration tests: route ↔ logic ↔ database seams, externals faked at the boundary.
- Mirrors the project's existing framework and conventions, or sets up the stack standard.
- Asserts caller-observable behavior, never private internals.

**Guardrail:** no filler tests for coverage theater, no asserting around known bugs, and
untestable code gets reported as a design defect instead of contorted around.

Trigger phrases: runs after `construct` in every inspection phase; also "write tests for
this", "add integration tests".
