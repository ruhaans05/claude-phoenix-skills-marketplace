# db-consult

The city-archivist's decision phase — and the only point in the whole pipeline allowed to
interrupt the user. Determines whether persistence is genuinely needed, recommends the
smallest engine that fits, and batches every user-only question (engine preference,
credentials, API keys, existing-DB constraints) into one exchange.

**Why it helps:** the pipeline's autonomy is the product. Database setup is the one phase
that can genuinely require the user — so it asks once, asks everything, and never comes
back.

**What it does:**
- Verifies the need is real (cache/config/process-lifetime state ≠ database) and
  recommends in-memory when that honestly serves the definition of done.
- Decides what convention can decide: SQLite → Postgres → hosted, smallest fit first.
- Batches user-only questions with safe defaults listed where they exist.
- Hands the resolved decisions to `db-provision`.

**Guardrail:** one exchange maximum; never invents credentials (no answer + no safe
default = reported blocker); treats received credentials as secrets from the first
moment; never upsells past the user's stated choice.

Trigger phrases: work order names a database, or Mayor summons the archivist on an
inferred persistence need.
