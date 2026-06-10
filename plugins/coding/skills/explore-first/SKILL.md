---
name: explore-first
description: >
  Map the relevant code and existing patterns BEFORE writing, so you reuse what's there
  instead of duplicating it. Spawn read-only explore agents to locate code, then plan against
  what you found. Use when starting in an unfamiliar codebase, before adding a feature, or
  when the user says "where does X live", "understand this code", "explore before changing".
---

# Explore First

The most common agentic-coding failure is writing code the project already has, in a style
the project doesn't use. Fix it by exploring before editing. A few minutes of read-only
search saves a wrong-shaped change and a redo.

## The loop

1. **State what you need to know** — the 2–4 concrete questions whose answers unblock the
   task. e.g. "Where are HTTP routes registered? How is auth applied? Is there a validation
   util already?"
2. **Search read-only first** — Grep/Glob/Read, or spawn an Explore/investigator subagent
   for broad fan-out. Find: the entry points, the existing pattern for this kind of change,
   and any utility you'd otherwise reinvent.
3. **Record findings as `file:line` facts**, not raw dumps. e.g. "routes registered in
   `server/router.ts:30`; auth via `requireAuth` middleware `auth/mw.ts:12`; validation
   helper `lib/validate.ts:`."
4. **Plan against reality** — design the change to fit the discovered pattern and reuse the
   discovered utilities. Only then edit.

## Parallelize the search

When scope is uncertain or spans areas, spawn explore agents **in parallel** (one message,
multiple calls) with distinct focuses: one finds the feature's entry point, one finds the
existing pattern to copy, one finds tests/fixtures. Read-only — safe to fan out.

## What to look for

- The **existing way** this kind of thing is done (copy it, don't invent a second way).
- **Reusable utilities** — validation, errors, logging, HTTP, DB access.
- **Conventions** — naming, file layout, error handling, comment density.
- **Tests** — where they live, how they're run, fixtures to mirror.
- **Entry points and call paths** for the code you'll touch.

## Guardrails

- Don't boil the ocean — explore only what the task needs; stop when the questions are
  answered.
- Don't edit during exploration. Separate "understand" from "change".
- Prefer the index/search tools over re-reading whole files; record the conclusion, drop the
  dump (pairs with `context-prune`).
- Findings are facts to verify, not assumptions — confirm a symbol exists before building on
  it.
