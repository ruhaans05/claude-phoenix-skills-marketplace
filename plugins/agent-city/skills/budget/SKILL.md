---
name: budget
description: >
  The city-bank's continuous, advisory-only watch on token spend. Rides every phase
  read-only: estimates and logs token cost per phase to the ledger, spots waste
  (re-reading what the ledger already holds, re-deriving the blueprint, whole-file reads
  where a span would do, redundant tool calls), and surfaces optimizations that cost
  nothing in correctness or safety. Never halts, never slows, never weakens a test or a
  marshal check. Use to track or reduce a run's token cost without hindering the work.
---

# budget

A meter on the run, not a brake. The bank watches what each phase spends, writes it down,
and — only when there's a real, free saving — tells the Mayor how the same result could
cost less. The Mayor is free to ignore every word of it. Nothing in this skill can stop,
pause, or slow a phase.

## The unbreakable rule

**No suggestion may hinder the technical process.** A saving that skips a test, shortens
the inspector's run, starves the engineer of context it genuinely needs, weakens a marshal
checkpoint, or lowers the work order's quality bar is not a saving — it's a regression, and
the budget never proposes it. Every optimization here is "same work, less waste," never
"less work."

## What it does each phase

1. **Meter.** Estimate the phase's token spend (input + output + tool calls). Mark
   estimates as approximate when they can't be measured exactly.
2. **Log.** One ledger line: `budget — <phase>: ~N tok (est) · note`. A lean phase gets
   the line and nothing more.
3. **Spot waste.** Look only for cost that bought nothing:
   - re-reading files already summarized in the ledger or blueprint
   - re-deriving a plan an earlier phase already recorded
   - pulling whole files into context where the needed span is a few lines
   - duplicate tool calls fetching the same data
   - a re-survey of a module the diff never touched
4. **Advise.** When the saving is real *and* free, surface it to the Mayor as a concrete
   suggestion. When it isn't, stay quiet.

## The optimization playbook (all correctness-free)

| Waste | Free fix |
|-------|----------|
| Re-reading the codebase a prior phase already learned | Reuse the ledger / blueprint record |
| Main thread spending big tokens to locate or survey code | Delegate to a compressed subagent; consume its summary |
| Whole-file reads for a small change | Read the spans the phase needs |
| Diffing or re-checking unchanged modules | Scope to what actually changed |
| Two calls fetching the same thing | Drop the duplicate, keep the necessary pass |

## Guardrails

- **Advisory only.** No halt, no veto, no gate, no slowing. The bank informs; the Mayor
  decides; an ignored suggestion is a fine outcome.
- **Safety and correctness outrank the budget, always.** If advice ever seems to conflict
  with a marshal check or the inspector's coverage, the check and the coverage win and the
  suggestion is withdrawn.
- **Honest numbers.** Estimates labeled as estimates; never an invented figure.
- **Read-only.** No edits, pushes, or provisioning — the bank observes and reports.
- **Don't nag.** A suggestion earns its interruption only if the saving is real and free;
  otherwise one quiet ledger line is the whole job.
