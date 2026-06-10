---
name: safe-refactor
description: >
  Improve code structure WITHOUT changing behavior, in small verified steps under a test
  safety net. Establish green tests first, refactor in reversible increments, re-run after
  each. Use when restructuring/renaming/extracting/deduping, or when the user says "refactor
  this", "clean this up", "extract a function", "simplify", "DRY this".
---

# Safe Refactor

Refactoring changes structure, not behavior. The risk is silently breaking something while
"improving" it. Safety comes from a test net plus small reversible steps — not from being
careful in one big leap.

## Before touching anything

1. **Establish a safety net.** Find and run the relevant tests; confirm green. No tests for
   this code? Add characterization tests that capture current behavior first (hand to
   `tdd-loop`), or proceed only with explicit run-the-app verification after each step.
2. **Define the target.** Name the specific smell and the end shape: "extract duplicated
   validation in `a.ts`/`b.ts` into `lib/validate.ts`." Vague "make it nicer" drifts.

## The loop

1. One small transformation (rename, extract, inline, move, dedupe). Behavior-preserving by
   construction.
2. Run the tests / the app. Still green → keep. Red → revert this step, it changed behavior.
3. Commit-sized chunks: each step independently sound and reversible.
4. Repeat toward the target. Stop when the smell is gone — don't gold-plate.

## Good refactors

- Extract a well-named function from a long one; inline a needless indirection.
- Dedupe by reusing an existing util (find it via `explore-first`) — don't add a third copy.
- Rename for clarity; narrow a type; remove dead code.
- Match the surrounding style — a refactor that fights the file's idioms isn't an improvement.

## Guardrails

- **Never mix behavior changes into a refactor.** If you must fix a bug, do it as a separate,
  labeled step — don't smuggle it in.
- Don't refactor untested code without first pinning its behavior, or you can't tell if you
  broke it.
- Keep the public interface stable unless the task is explicitly to change it; if it must
  change, update all callers in the same pass.
- Re-run the net after every step, not just at the end — small blast radius = fast diagnosis.
