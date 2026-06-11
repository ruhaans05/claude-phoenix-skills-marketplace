---
name: construct
description: >
  The city-engineer's build phase: implement the blueprint in small increments, verifying
  each one compiles/imports/runs before the next. Also the pipeline's repair bay — every
  red test and rejected PR comes back here with a diagnosis to fix. Use when a blueprint
  exists and code needs to be written, or when a failure diagnosis needs repairing.
---

# Construct

Execute the blueprint. The thinking happened in `blueprint`; this phase turns it into
working code, one verifiable increment at a time.

## The build loop

1. Take the next increment from the blueprint's build order.
2. Write it — matching the surrounding code's naming, idioms, error handling, and comment
   density. New code should look like it grew there.
3. Sanity-check immediately: compile / import / run the entry point / exercise the piece
   directly. Catch breakage at one increment's distance, not at inspection.
4. Increment works → next. Doesn't → fix now; never stack new work on a broken base.

## Repair mode (red test or PR rejection routed back)

1. Read the diagnosis — the inspector's root cause or the courier's review analysis.
2. Reproduce it yourself: run the failing test, hit the failing path. Never fix what you
   haven't seen fail.
3. Fix the **cause** named in the diagnosis, not the symptom. If reproduction shows the
   diagnosis is wrong, report what you found instead — with evidence — rather than
   patching around it.
4. Re-run what failed, plus anything adjacent your fix could plausibly have touched.

## Rules

- **Blueprint deviations get recorded.** Reality beats the plan sometimes; when it does,
  note what changed and why, so inspection and the PR description stay truthful.
- **Tests are not yours to edit.** If a test seems wrong, report the reasoning; the
  inspector rules on it. Changing a test to make your code pass hides the bug you were
  sent to fix.
- **Real persistence need with no DB provisioned → stop and flag the Mayor.** No ad-hoc
  JSON-file storage to dodge the archivist.
- **No new dependencies beyond the blueprint** without recording the justification.
- **Secrets never appear in code.** Config via environment variables; anything
  credential-shaped hardcoded is a defect, even temporarily.

## Guardrail

Never report "built" with a known-broken increment in the pile, and never widen the
change beyond blueprint + work order. Done means: every increment sanity-checked, ready
for the inspector to verify for real.
