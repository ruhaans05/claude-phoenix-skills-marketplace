# blueprint

The city-engineer's design phase: survey the codebase read-only, then draft a concrete
implementation plan — files, interfaces, data flow, reuse targets, build order — before a
single line is written.

**Why it helps:** code written blind duplicates existing utilities and fights the repo's
conventions. A blueprint makes `construct` pure execution, and catches the "this needs a
database" discovery while it's still cheap to handle.

**What it does:**
- Maps layout, conventions, and reusable code as `file:line` facts.
- Produces a structured blueprint with small, independently verifiable increments.
- Flags persistence needs to the Mayor before construction starts.
- On fix-up passes, shrinks to a mini-blueprint: cause → intended fix → build.

**Guardrail:** strictly read-only until the blueprint exists — no scaffolding, no head
starts. And the blueprint never grows past the work order.

Trigger phrases: runs at the start of every engineering phase, before `construct`.
