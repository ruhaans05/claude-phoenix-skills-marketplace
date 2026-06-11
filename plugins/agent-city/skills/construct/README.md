# construct

The city-engineer's build phase: implements the blueprint increment by increment, sanity-
checking each piece before the next. Doubles as the pipeline's repair bay — red tests and
PR rejections come back here with a diagnosis attached.

**Why it helps:** small verified increments catch breakage one step from where it
happened, instead of letting it surface as a pile of red at inspection. Repair mode fixes
the *diagnosed cause*, not the symptom.

**What it does:**
- Builds in the blueprint's order, matching the surrounding code's style and idioms.
- Compiles/imports/runs each increment before stacking the next on top.
- In repair mode: reproduce the failure first, fix the cause, re-run what failed.
- Records any deviation from the blueprint so reports stay truthful.

**Guardrail:** never edits tests to pass them, never hand-rolls storage to dodge the
archivist, never hardcodes secrets, never claims "built" over a known-broken increment.

Trigger phrases: runs after `blueprint` in every engineering phase, and on every failure
routed back by the inspector or courier.
