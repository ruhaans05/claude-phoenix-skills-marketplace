# patrol

The city-marshal's checkpoint sweep: at every phase transition, a fast pass/fail check
that the run is ethical, legal, and inside the city's own laws. Silent when clean — one
ledger line per checkpoint — loud the moment it isn't. The chief makes one sweep and
splits it across four deputies.

**Why it helps:** an autonomous pipeline without oversight optimizes for "checks pass",
not "should exist". Patrol is the structural answer: a read-only observer with halt
authority, separate from every agent that builds, so compliance is checked by someone
with nothing to gain from looking away. Splitting it into deputies keeps each beat
focused — and makes every flag say exactly which kind of problem it found.

**The four deputies:**
- **ethics** — should this exist, and is it honest? Malware, credential harvesting,
  deceptive software, and ToS-violating scraping end the run on the spot. Owns the hard
  lines.
- **licenses & policy** — license compatibility and attribution, copyleft contamination,
  dependency-policy fit, stripped notices.
- **secrets & PII** — credential-shaped strings in a diff, plaintext secrets, `.env`
  hygiene, personal data beyond need, regulated data handled casually.
- **data quality** — integrity and honest claims: replayable migrations, no silent data
  loss, no fabricated results, plus the city's own laws (no merge to main, no unverified
  claims, no buried failures).

**Guardrail:** every flag carries evidence (file:line, license clause, ToS section) and
names its deputy, never vibes; scrutiny scales with stakes; judgment calls go to the user
with the facts — only the deputy of ethics' hard violations are non-negotiable. Patrol
never silently rewrites work to make it pass.

Trigger phrases: rides every phase automatically; also "is this compliant", "check the
licenses", "is the data handling okay", "is this okay to build".
