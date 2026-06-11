---
name: pr-open
description: >
  The city-courier's delivery phase: take inspected work to GitHub — feature branch off
  the default branch, clean commits, push, open the pull request with an honest
  description. Never targets, pushes to, or merges main. Use when the inspection report
  says PASS and the change needs to become a PR, or when the user asks to "open a PR" /
  "push this up".
---

# PR Open

Deliver the work as a pull request. The PR is the pipeline's product — main belongs to
the humans.

## The delivery

1. **Branch.** From the up-to-date default branch, a descriptive feature branch:
   `agent-city/<short-kebab-summary>`. Never work directly on the default branch; if you
   discover you somehow are, move the work to a branch before anything else.
2. **Commit.** Stage deliberately — review what's being staged; build artifacts, stray
   files, and anything `.env`-shaped stay out. Clean conventional messages: subject ≤50
   chars stating the change; body only where the why isn't obvious.
3. **Pre-flight.** Last sweep of the diff for anything credential-shaped (keys, tokens,
   connection strings). Found → stop, remove it, tell the Mayor. A pushed secret is
   published; this is the final gate where it's still cheap.
4. **Push** the feature branch to the remote. No remote configured → report to the Mayor
   (a repo/remote decision is the user's); don't invent one.
5. **Open the PR** against the default branch (`gh pr create` or the platform
   equivalent), and record its URL — the steward and the final report need it.

## The PR description

```markdown
## What
<what was built, in work-order terms>

## Why
<the work order's intent / definition of done>

## How verified
<the inspector's real numbers: suite, passed/failed/skipped, integration coverage>

## Database
<provisioned: engine, migration files, .env.example pointer | none>

## Notes for the reviewer
<deviations from blueprint, convention choices made, known limitations>
```

Every claim in it traceable to something that actually ran. A misled reviewer is a
rejection earned — and rejections cost iterations.

## Guardrails

- **Never push to main, never merge.** The PR's terminal state in this pipeline is open
  and passing; crossing into main is the human's click, not yours.
- Inspection verdict was FAIL and the work order says one-pass → the PR may still open,
  but the description must lead with what's red. Never dress a failing change as ready.
- Repo has CONTRIBUTING/PR-template conventions → follow them over this template's shape
  (keep the honesty rules either way).
