---
name: blueprint
description: >
  The city-engineer's design phase: survey the existing codebase read-only, then draft the
  implementation plan — files to create or touch, interfaces, data flow, what existing
  code to reuse, and whether persistence is needed. Runs before construct, every time,
  including fix-up passes. Use when a work order arrives and before any code is written.
---

# Blueprint

Code written blind duplicates what already exists and fights the codebase's own
conventions. The blueprint is a short, concrete plan drawn *after* looking — so
`construct` is execution, not exploration.

## Survey (read-only)

- Map the territory: project layout, language, framework, package manifest, how it runs,
  how it's tested. In a fresh repo, this is fast; record it anyway.
- Hunt for reuse: utilities, services, patterns the new code should call instead of
  reinvent. Record findings as `file:line` facts, not impressions.
- Find the conventions: naming, error handling, module boundaries, comment density. The
  new code should be indistinguishable from a careful native's.

## Draft

```markdown
## Blueprint
- **Change:** <one sentence, tied to the work order's definition of done>
- **Files:** <each file to create or modify, and what changes in it>
- **Interfaces:** <signatures/endpoints/schemas the change exposes or consumes>
- **Data flow:** <where state lives, what flows where>
- **Reuse:** <existing code this calls, with file:line>
- **Persistence:** <none | uses provisioned DB | NEEDS DB — flag to Mayor now>
- **Build order:** <increments, each one verifiable on its own>
- **Risks:** <what's most likely to go wrong; what to check first>
```

## Rules

- **Persistence is flagged here**, not discovered mid-construct. If the design needs a
  database and none is provisioned, the flag goes to the Mayor before building starts —
  the archivist's consult is far cheaper before construction than after.
- **Increments must be small and independently checkable.** "Build the whole app" is not
  a build order; "1. data model, 2. core logic + its seam, 3. HTTP layer, 4. wiring" is.
- **Fix-up passes get a mini-blueprint:** re-read the failing area, state the cause and
  the intended fix in one or two lines, then construct. Diagnosis before edits, always.
- **Plan within the work order.** If surveying reveals the request is bigger than it
  looked, report that to the Mayor — don't silently grow the blueprint.

## Guardrail

Read-only until the blueprint exists. No edits, no scaffolding, no "small head start" —
the survey loses its honesty the moment writing begins.
