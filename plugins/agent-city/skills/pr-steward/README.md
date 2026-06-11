# pr-steward

The city-courier's stewardship phase: watches the open PR until it passes — polls CI,
reads failing logs (never guesses from check names), reads review rejections for their
*reasoning*, fixes mechanical issues directly, and routes substantive ones back through
the engineer + inspector loop.

**Why it helps:** an opened PR isn't a delivered PR. Checks fail, reviewers reject — the
steward turns each into a classified diagnosis and a routed fix, so the pipeline
converges on green instead of stalling at the first red X.

**What it does:**
- Polls checks and review state; classifies every red: mechanical / substantive / flake /
  blocked.
- CI-only failures get an environment diff (versions, env vars, services) as the diagnosis.
- Restates each rejection in its own words before acting — understanding gate.
- Responds on the PR after each fix: what changed, which commit, which comment.

**Guardrail:** never merges, never pushes to main, never shortcuts re-inspection to clear
a thread, never force-pushes after human review, respects the Mayor's iteration cap.

Trigger phrases: runs after `pr-open`; also "check on the PR", "CI is failing", "the
review came back".
