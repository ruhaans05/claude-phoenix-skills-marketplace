# no-destruction

Inserts a deliberate stop before any hard-to-reverse action — deleting/overwriting files,
force-pushing, dropping tables, writing to production, mass edits — and inspects the target
before anything is destroyed.

**Why it helps:** the cost of a wrong destructive action is far higher than the cost of one
confirmation. This skill makes the agent look before it leaps and prefer the reversible path.

**What it does:**
- Flags the full set of destructive operations (shell, git, SQL, infra, bulk).
- Requires inspecting/dry-running the target first, and stops if what's found contradicts how
  it was described.
- Confirms intent unless durably authorized, and prefers reversible alternatives (trash,
  soft-delete, backup, feature branch).

**Guardrail:** never widens blast radius for convenience, never `--force`s past a safety
check, treats an unverified backup as no backup.

Trigger phrases: "delete", "drop", "force push", "reset --hard", "overwrite", "wipe".
