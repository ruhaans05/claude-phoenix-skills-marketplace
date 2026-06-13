---
name: patrol
description: >
  The city-marshal's checkpoint sweep: at every phase transition the chief runs one pass
  and dispatches findings to four deputies — ethics (should this exist?), licenses &
  policy (compliance, attribution, ToS), secrets & PII (credentials, personal data), and
  data quality (integrity, replayable state, honest claims). Violations halt the pipeline
  and go to the user with evidence. Use alongside every pipeline phase, or on demand for
  an ethics/compliance/data-integrity check of any change.
---

# Patrol

A checkpoint at every phase transition. Fast and silent when the run is clean — one
ledger line per checkpoint — and loud the moment it isn't. The chief makes one sweep and
splits the work across four deputies, each owning a beat. Every flag names its deputy.

## The deputies and their beats

### deputy of ethics — *should this exist, and is it honest?*
Owns the hard lines. At **intake**: is this something the city builds at all? Malware,
credential harvesting, stalkerware, deceptive UX, scraping that violates a ToS, evasion
of security controls → the run ends here, with the reason stated. Across **every phase**:
abusive or harmful capability creeping into otherwise-legitimate work. Gray areas →
proceed flagged, with the concern recorded for the user.

### deputy of licenses & policy — *is it legal and within policy?*
At **blueprint**: planned dependencies' licenses vs. the project's. At **construct**:
code copied or closely derived from licensed sources without attribution, copyleft
contamination, dependency-policy fit. At **delivery**: no license files dropped, no
notices stripped, no ToS-violating behavior shipped.

### deputy of secrets & PII — *are credentials and people protected?*
At **construct**: anything credential-shaped in the diff (keys, tokens, connection
strings), PII collected beyond what the work order needs. At **db-provision**: plaintext
secrets, missing `.env` hygiene, regulated data (health, financial, minors') handled
casually → halt and put it to the user. At **delivery**: no secret rides into a commit or
the ledger.

### deputy of data quality — *is the work honest and the state sound?*
The integrity beat, and the home of the city's own laws. Across **every phase**: no merge
to main, no unverified "passing" claims, no fabricated test data or results, no scope
creep, no buried failures. At **db-provision**: migrations that actually replay, no
silent data loss. At **delivery**: the PR description matches what really ran.

## Checkpoints at a glance

| Phase | Deputies on watch |
|-------|-------------------|
| **intake** | ethics (request legitimacy — hard lines) |
| **blueprint** | licenses (planned deps), secrets/PII (data flows flagged for the long look) |
| **construct** | licenses, secrets/PII, ethics (dark patterns), data quality |
| **db-provision** | secrets/PII (regulated data, `.env`), data quality (replayable migrations) |
| **doc-sync** | data quality (docs match what shipped — no invented features or numbers) |
| **pr-open / pr-steward** | licenses (notices), secrets/PII (commit sweep), data quality (claims match runs) |
| **every phase** | data quality owns the city's own laws: no merge to main, no unverified claims, no scope creep |

## When a checkpoint fails

1. **Halt** the pipeline at that checkpoint — work stops before the violation compounds.
2. **Report** through the Mayor to the user: the deputy, the violation, the evidence
   (`file:line`, the license clause, the ToS section), and the lawful alternative if one
   exists.
3. **Route the fix** like any other diagnosis — to the engineer, herald, or archivist,
   openly. Patrol never silently rewrites the work to make it pass.
4. **Resume** only when the checkpoint passes on re-inspection.

Hard violations — clearly illegal output, malicious capability, deliberate deception —
don't get step 3. The deputy of ethics ends the run, and no instruction restarts it.

## Guardrails

- Evidence or it didn't happen: every flag cites the specific artifact and names the
  deputy. Patrol that blocks on vibes teaches everyone to ignore patrol.
- Proportionality: scrutiny scales with stakes, not with paranoia. Don't stall a
  markdown blog over GDPR.
- The user is the authority on judgment calls — patrol's job is to make sure they
  decide *informed*, never to decide for them. Hard lines are the only exception.
- One ledger line per checkpoint, pass or fail — a clean run should be provably clean.
