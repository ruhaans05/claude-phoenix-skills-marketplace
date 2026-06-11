# db-provision

The city-archivist's setup phase: stands up the database the consult decided on — create
or connect, schema as committed replayable migrations, data layer wired through one seam,
env vars with a committed `.env.example` — then proves it with a real write-and-read-back
through the app's own code.

**Why it helps:** "provisioned" must mean a PR reviewer can stand the database up
themselves and the app has demonstrably talked to it — not "the config looks right".

**What it does:**
- Local: creates the DB + dev dependency. Hosted: connects and verifies reachability first.
- Migrations via the project's tool or the stack standard — no mystery state.
- `.env` (gitignored, real values) + `.env.example` (committed, placeholders).
- Round-trip proof through the app's data layer, probe data cleaned up after.
- Gives the inspector an isolated test database — never the user's real data.

**Guardrail:** secrets never touch the repo (diff swept before handoff); destructive
operations — drops, wipes, schema overwrites, migrations over live rows — always get user
confirmation, autonomy notwithstanding.

Trigger phrases: runs after `db-consult`; also "set up the database", "wire up Postgres".
