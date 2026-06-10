---
name: doc-sync
description: >
  Keep docs true to the code after a change: find documentation that the change made stale —
  docstrings, README, API docs, comments, examples — and update it to match the new behavior.
  Use after editing code, before committing, or when the user says "update the docs", "are the
  docs still right", "the docs are stale", "sync docs".
---

# Doc Sync

Stale docs are a silent bug: the code changed, the doc didn't, and now the doc lies. The most
reliable moment to keep docs honest is right after the code changes — while you still know
exactly what changed.

## After a code change, check for drift

1. **Diff-driven search.** For each thing you changed — a function signature, a return shape,
   a flag, an endpoint, a default, a behavior — find every place it's documented:
   - Its own docstring/comments
   - README usage and examples
   - API/reference docs
   - CHANGELOG (does this change warrant an entry?)
   - Code examples and snippets anywhere in the repo
2. **Compare doc against new reality.** Where the doc now describes old behavior, it's stale.
3. **Update to match.** Fix the docstring, the example, the parameter list, the described
   behavior — so the doc states what the code now does.

## High-drift spots to always check

- Renamed/added/removed parameters → docstrings, API docs, examples.
- Changed return shape/type → docs that show or describe the output.
- Renamed function/endpoint/flag → every reference and example.
- Changed default value → docs quoting the old default.
- Removed feature → docs that still advertise it.
- Install/setup change → README quickstart.

## Guardrails

- A code change isn't done until its docs match — treat doc-sync as part of the change, not a
  later chore.
- **Verify the example still runs** after updating it; a "fixed" example that doesn't execute
  is still broken (pairs with `verify-before-done`).
- Don't silently delete docs to resolve drift — if the feature's gone, remove its docs and note
  it (CHANGELOG/`changelog-keep`); if it moved, update the reference.
- Stay in scope: update docs the change actually affected, not a wholesale docs rewrite (pairs
  with `scope-guard`).
- When you can't tell if a doc is stale or intentionally aspirational, surface it rather than
  guessing.
