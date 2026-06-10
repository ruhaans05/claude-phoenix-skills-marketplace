# scope-guard

Keeps a change inside what was actually asked — flagging unrelated edits, opportunistic
refactors, and drive-by reformatting, and surfacing them as follow-ups instead of bundling
them in.

**Why it helps:** a focused change is easy to review, revert, and trust. Scope creep hides
the real change, expands the blast radius, and breaks things the reviewer didn't expect.

**What it does:**
- Restates the boundary before editing and touches only what the task needs.
- Keeps refactor / feature / bug fix as separate commits.
- Reviews its own diff for drift — every hunk must trace back to the stated scope.

**Guardrail:** never expands scope without asking (even an obvious improvement — offer it as a
follow-up), never silently skips part of the asked change.

Trigger phrases: "stay in scope", "don't change unrelated things", "keep the diff focused",
"minimal change".
