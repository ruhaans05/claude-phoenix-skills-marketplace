---
name: scope-guard
description: >
  Keep a change inside what was actually asked. Flag scope creep — unrelated edits,
  opportunistic refactors, drive-by reformatting — and surface them instead of bundling them
  in. Use during multi-file changes, before committing, or when the user says "stay in
  scope", "don't change unrelated things", "keep the diff focused", "minimal change".
---

# Scope Guard

A focused change is easy to review, easy to revert, and easy to trust. Scope creep — fixing
unrelated things, reformatting whole files, refactoring while adding a feature — hides the
real change, expands the blast radius, and breaks things the reviewer didn't expect.

## Stay inside the asked change

1. **Restate the scope** before editing: the specific behavior/files the task requires. That
   is the boundary.
2. **Touch only what the task needs.** If a line isn't required for the goal, don't change
   it — including formatting, import reordering, and renames in untouched code.
3. **Spotted an unrelated problem?** Note it, don't fix it here. Surface it to the user as a
   follow-up. One unrelated bug fix in a feature diff is still scope creep.
4. **Keep change types separate.** A refactor, a feature, and a bug fix are three commits, not
   one. Don't smuggle a behavior change into a "cleanup".
5. **Review your own diff for drift** before committing — every hunk should trace back to the
   stated scope. If one doesn't, pull it out.

## Signs of scope creep to catch

- Reformatting / whitespace changes across files you only meant to touch in one spot.
- "While I'm here" refactors of code unrelated to the task.
- Renames that ripple far beyond the change.
- New abstractions/dependencies the task didn't call for.
- Auto-formatter rewriting an entire file when you changed two lines.

## Guardrails

- Don't expand scope without asking — even an obvious improvement. The user owns that
  decision; offer it as a follow-up.
- Don't shrink scope silently either — if you can't safely do part of the asked change, say
  so, don't just skip it.
- A minimal diff is a feature, not laziness. Prefer the smallest change that fully does the
  task.
- If a formatter wants to rewrite unrelated lines, scope its run to the changed files/region.
