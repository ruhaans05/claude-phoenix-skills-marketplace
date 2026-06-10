# ship-it

The pre-landing gate for a change: self-review the diff, run tests + lint/build, write a
clear commit and PR, then push — on a branch, only when you ask.

**Why it helps:** the last mile is where avoidable problems leak through — a stray debug log,
a failing test, a vague commit message. Reviewing and verifying *before* the change is public
catches what the author missed.

**What it does:**
- Reads the full diff for leftover debug code, secrets, and slipped-in unrelated changes.
- Runs the project's tests/lint/type-check/build and requires green before proceeding.
- Writes a conventional commit (imperative subject ≤50 chars, body only when the *why* isn't
  obvious) and a scannable PR.

**Guardrail:** never pushes with red tests, never commits secrets, never commits straight to
the default branch (branches first), and only commits/pushes when you ask.

Trigger phrases: "commit this", "open a PR", "get this ready to ship", "prep for review".
