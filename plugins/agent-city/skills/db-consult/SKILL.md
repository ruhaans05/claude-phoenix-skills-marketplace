---
name: db-consult
description: >
  The city-archivist's decision phase, and the pipeline's single sanctioned
  user-interruption point: determine whether a database is genuinely needed, what kind,
  and gather everything only the user can provide — engine choice, hosting, credentials,
  API keys — in one batched exchange. Use when the work order names a database, or when
  the Mayor summons the archivist because a phase reported a real persistence need.
---

# DB Consult

The pipeline promises the user peace after the initial command — except here. This skill
exists so that when the pipeline does interrupt, it interrupts **once**, with everything
batched, and never needs to come back.

## First: is a database genuinely needed?

- Work order names one → yes; the user's choice of engine wins outright.
- Work order opted out ("no database", "in-memory fine") → no consult; report back.
- Inferred mid-pipeline → verify the need is real: data must survive restarts, or be
  queried/shared/concurrent. A cache, a fixed config file, or process-lifetime state is
  **not** a database need. If in-memory honestly serves the definition of done, recommend
  that and skip the interruption entirely.

## Then: decide what you can, list what you can't

Decide by convention (and disclose in the consult): smallest engine that serves the work
order — SQLite for single-process local persistence; Postgres when concurrency, real
relational integrity, or production deployment demand it; a document/key-value store when
the data shape genuinely fits. Schema shape comes from the blueprint, not the user.

Only the user can supply:
- Engine/hosting **preference** that overrides your recommendation
- Credentials and connection strings for anything hosted/existing
- API keys for managed services (Supabase, Neon, PlanetScale, Atlas…)
- Whether a database already exists that this app must join, and its schema constraints

## The consult (one exchange)

```markdown
## Database Consult
**Need:** <why this build needs persistence, one sentence>
**Recommendation:** <engine + hosting, and why it's the smallest fit>

Questions (answer in one reply):
1. <engine/hosting — accept recommendation or name preference>
2. <credentials/connection string, IF a hosted/existing DB is in play>
3. <anything else blocking provision — each question lists its default-if-unanswered
   where a safe default exists>
```

Answers in hand (or defaults applied) → hand the decisions to `db-provision`.

## Guardrails

- **One exchange.** A second interruption means this consult failed — over-ask slightly
  rather than return.
- Never invent or assume credentials; no answer and no safe default = blocked, reported
  precisely to the Mayor.
- Credentials received are handled as secrets from the first moment: env vars only,
  never echoed back in full, never committed (see `db-provision`).
- Don't upsell. The user's "SQLite is fine" beats your architectural taste.
