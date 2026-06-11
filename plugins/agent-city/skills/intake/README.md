# intake

Parses the user's single build command into the work order that drives the entire
pipeline: what to build, definition of done, stack, database status, iterate-or-not, and
iteration cap.

**Why it helps:** the pipeline's promise is one command in, a PR out. That only works if
everything decidable is decided up front — extraction first, convention second — so later
phases never have to stop and ask.

**What it does:**
- Produces a structured work order every other agent builds against.
- Pins the definition of done as observable behavior, not vibes.
- Records the database situation: named, opted out, or "infer during build".
- Captures the iteration policy, quoting the user's exact opt-out phrase if present.
- Lists what's out of scope — the cheapest scope-creep prevention there is.

**Guardrail:** never interrogates the user (database questions belong to the archivist's
one sanctioned consult), and never expands the request beyond what was asked.

Trigger phrases: runs first in every "build me X" / "create and ship Y" pipeline.
