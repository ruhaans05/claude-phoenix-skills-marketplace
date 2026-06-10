---
name: context-prune
description: >
  Identify and drop stale, dead, or superseded context (old tool dumps, replaced file reads,
  abandoned exploration) so every following turn pays fewer input tokens. Keeps what informs
  the next action. Use when context is bloated, a run is long/expensive, or the user says
  "prune context", "trim the context", "too many tokens", "drop stale output", "compact".
---

# Context Prune

Input tokens are re-paid on every turn — a bloated context taxes the entire rest of the run.
Pruning removes what no longer informs the next action so the cost stops compounding.

## Signs context is bloated

- Large raw tool outputs from early turns you've already acted on
- Multiple reads of the same file (only the latest matters)
- A full file dumped when you only needed three functions
- Exploration paths you abandoned (searches that led nowhere)
- Verbose logs/build output where only the final status matters
- Long pasted data already summarized into a conclusion

## Safe to drop

- Stale tool results whose conclusion you've already used and recorded
- Superseded file reads (file was re-read or edited since)
- Dead-end search output that didn't change the plan
- Intermediate scratch work after the decision is made
- Redundant duplicates of the same content

## Unsafe to drop — keep

- Anything you're actively reasoning over right now
- The current task spec / acceptance criteria
- The latest state of files you're editing
- Decisions and their rationale (the *why*, even after dropping the raw input)
- Error messages for bugs still open
- User instructions and constraints

## How to prune

1. Before a new phase, scan context for the "Safe to drop" patterns.
2. Replace bulky raw output with a one-line conclusion: keep the *finding*, drop the *dump*.
   e.g. "grep across src/ → auth handled only in `auth/middleware.ts:42`" replaces the full
   grep output.
3. Don't re-read files you already hold current — re-reading re-adds the tokens you pruned.
4. In harnesses with auto-compaction, this skill is the manual, surgical complement: prune
   intentionally at phase boundaries instead of waiting for a blunt auto-summary.

## Guardrails

- Never prune mid-reasoning — losing context you need forces a re-read that costs more.
- When unsure whether a fact still matters, keep the one-line conclusion, drop only the bulk.
- Preserve correctness over savings. A dropped constraint that causes a wrong answer is the
  most expensive mistake of all.
