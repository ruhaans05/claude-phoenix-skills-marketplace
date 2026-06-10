---
name: no-destruction
description: >
  Stop before any hard-to-reverse action and confirm intent first: deleting or overwriting
  files, force-pushing, dropping/truncating tables, writing to production, mass edits.
  Inspect the target before destroying it. Use before destructive shell/git/SQL commands, or
  when the user says "delete", "drop", "force push", "reset --hard", "overwrite", "wipe".
---

# No Destruction

Destructive actions are the ones you cannot take back. The cost of a wrong destructive action
is far higher than the cost of one confirmation. This skill inserts a deliberate stop before
anything irreversible.

## Treat as destructive (stop and confirm first)

- File deletion or overwrite: `rm`, `rm -rf`, `mv` onto an existing path, `>` truncation,
  writing a file you have not read.
- Git history rewrites: `git push --force`, `git reset --hard`, `git rebase` on shared
  branches, `git clean -fdx`, branch/tag deletion.
- Database: `DROP`, `TRUNCATE`, `DELETE`/`UPDATE` without a `WHERE`, schema migrations that
  drop columns/tables.
- Infrastructure / production: deploys, `terraform apply`/`destroy`, restarts, deleting cloud
  resources, secret rotation.
- Bulk operations: mass find-and-replace, recursive chmod/chown, deleting many files at once.

## The check, in order

1. **Inspect the target first.** Read the file, list the rows, show the diff, dry-run the
   command (`--dry-run`, `git status`, `SELECT` before `DELETE`). Know exactly what will be
   destroyed.
2. **Contradiction stop.** If what you find doesn't match how it was described, or you didn't
   create it and weren't clearly told to remove it, **stop and surface that** instead of
   proceeding.
3. **Confirm intent** unless the user already gave explicit, durable authorization for this
   specific action. Approval for one delete does not authorize the next.
4. **Prefer the reversible path.** Move to trash over `rm`; soft-delete over hard-delete;
   back up before overwrite; new migration over destructive one; feature branch over
   force-push to shared history.
5. Only then run it — and report exactly what was destroyed.

## Guardrails

- Never widen a destructive command's blast radius for convenience (`rm -rf .` instead of
  naming files).
- Never `--force` past a safety check (git hook, `--no-verify`, overwrite prompt) to "get
  it done".
- A backup that you haven't verified exists is not a backup. Confirm it before relying on it.
- When uncertain whether something is safe to destroy, assume it is not.
