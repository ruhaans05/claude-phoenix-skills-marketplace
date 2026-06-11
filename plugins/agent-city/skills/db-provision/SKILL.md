---
name: db-provision
description: >
  The city-archivist's setup phase: stand the decided database up — create or connect it,
  define the schema as committed migrations, wire the app's data layer, configure
  environment variables with a committed .env.example — and prove it with a real
  write-and-read-back through the app's own code. Use after db-consult has settled the
  decisions.
---

# DB Provision

The consult decided; now make it real. Provisioned means a reviewer can stand the
database up from the PR alone, and the app has demonstrably talked to it.

## The setup

1. **Create or connect.** Local engine: create the database/file, ensure the dev
   dependency is in the project manifest. Hosted: connect with the consult's credentials;
   verify reachability before building anything on top.
2. **Schema as migrations.** Committed, ordered, replayable migration files — the
   project's existing migration tool if there is one, the stack's standard (Alembic,
   Prisma, golang-migrate, Flyway…) if not. Schema shape comes from the blueprint's data
   model. No mystery state that exists only on your machine.
3. **Wire the data layer.** Connection setup where the project's conventions put config;
   the engineer's code reaches the DB through one seam (module/repository/client), not
   scattered raw connections.
4. **Configure env.**
   - Real values → `.env` (and ensure `.env` is gitignored — add it if missing).
   - Committed → `.env.example` with placeholder keys and a comment per variable.
   - README/PR notes gain the one-liner to set up: copy example, fill values, run
     migrations.
5. **Prove it.** Through the app's own data layer: write a record, read it back, confirm
   equality. Then clean up the probe data. Config that "looks right" is not provisioned.

## Reporting

To the Mayor (and into the PR description's Database section): engine + hosting,
migration file paths, env vars required (names only, never values), the seam module, and
the round-trip proof's result.

## Guardrails

- **Secrets never touch the repo.** Before handing off, sweep the diff for anything
  credential-shaped; a connection string in committed code is a defect that stops the
  phase.
- **Destructive operations need user confirmation, every time** — dropping tables,
  wiping data, overwriting an existing schema, replaying migrations on a database with
  live rows. Autonomy never covers destruction.
- Connecting to an **existing** database: read-first; learn its schema before writing;
  schema changes to it are migrations like any other, confirmed if they touch data.
- Test database for the inspector: same engine, isolated instance/schema — never the
  user's real data.
