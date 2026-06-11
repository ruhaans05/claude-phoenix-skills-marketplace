# pr-open

The city-courier's delivery phase: feature branch off the default branch, deliberate
clean commits, secret-sweep pre-flight, push, and a pull request whose description is
traceable to what actually ran. Never touches main.

**Why it helps:** the PR is the pipeline's product. A clean branch, honest description,
and verified test numbers give the human reviewer everything needed to decide the one
thing the pipeline never decides: merging.

**What it does:**
- Branches as `agent-city/<summary>`; moves work off the default branch if found there.
- Stages deliberately — no build artifacts, no stray files, nothing `.env`-shaped.
- Final secret sweep of the diff before pushing (last cheap place to catch one).
- Opens the PR with What / Why / How verified / Database / Reviewer notes.

**Guardrail:** never pushes to main, never merges, never dresses a failing change as
ready — a one-pass PR with red tests leads with what's red.

Trigger phrases: runs after an inspection PASS; also "open a PR", "push this up".
