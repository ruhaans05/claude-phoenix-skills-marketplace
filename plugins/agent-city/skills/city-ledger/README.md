# city-ledger

The city's public record: one file, `.agent-city/ledger.md`, committed on the feature
branch. The work order, every phase transition, every routed failure, every decision
made by convention — plus an always-current Status block.

**Why it helps:** two things kill autonomous runs — sessions that end mid-flight and
restart from zero, and PRs that arrive with no account of how they were made. The ledger
fixes both: a future session resumes from the Status block, and the PR reviewer reads
the full audit trail right in the diff.

**What it does:**
- Opened by `intake` with the work order verbatim; appended at every phase transition.
- One-line entries: timestamp, phase, outcome — real counts, real PR numbers, real
  diagnoses.
- Rides the feature branch, dies with a rejected PR, never touches the default branch
  on its own.
- Resume protocol: read Status, verify against git/GitHub reality, continue from the
  verified phase. Reality wins every disagreement.

**Guardrail:** never records unobserved results, never contains anything
credential-shaped (it's committed), and never overrides the real world — a stale ledger
gets corrected, not obeyed. Opt out with "no ledger" in the prompt.

Trigger phrases: opened at intake; appended every phase; read by `/city-status` and by
`/city` resuming a run in flight.
