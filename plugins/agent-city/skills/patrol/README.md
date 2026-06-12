# patrol

The city-marshal's checkpoint sweep: at every phase transition, a fast pass/fail check
that the run is ethical, legal, and inside the city's own laws. Silent when clean — one
ledger line per checkpoint — loud the moment it isn't.

**Why it helps:** an autonomous pipeline without oversight optimizes for "checks pass",
not "should exist". Patrol is the structural answer: a read-only observer with halt
authority, separate from every agent that builds, so compliance is checked by someone
with nothing to gain from looking away.

**What it checks:**
- intake — the request itself: malware, credential harvesting, deceptive software, and
  ToS-violating scraping end the run on the spot
- construct — license compliance and attribution, secrets in the diff, PII beyond need,
  dark patterns
- db-provision — data protection, plaintext secrets, regulated data handled casually
- delivery — pushes only to repos the user controls; PR claims match what actually ran
- always — the city's laws: no merge to main, no unverified claims, no buried failures

**Guardrail:** every flag carries evidence (file:line, license clause, ToS section),
never vibes; scrutiny scales with stakes; judgment calls go to the user with the facts —
only hard violations are non-negotiable. Patrol never silently rewrites work to make it
pass.

Trigger phrases: rides every phase automatically; also "is this compliant", "check the
licenses", "is this okay to build".
