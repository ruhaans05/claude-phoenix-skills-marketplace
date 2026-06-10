# model-router

Routes simple, mechanical subtasks to a cheaper/faster Claude model (Haiku) and keeps the
expensive frontier model (Opus) for the hard reasoning that actually needs it.

**Why it saves money:** in a typical agent run, most subtasks — finding code, renaming
things, generating boilerplate, summarizing one file — are mechanical. Running those on the
top-tier model is overkill. Haiku does them for a fraction of the cost and returns faster.

**What it does:**
- Gives clear rules for which subtasks go to Haiku vs stay on Opus.
- Shows the mechanic: the Agent tool's `model` parameter (`haiku` / `sonnet` / `opus`).
- Keeps a current model-tier table.

**Guardrail:** never downgrades a reasoning-heavy task just to save tokens — a wrong answer
costs more than it saves.

Trigger phrases: "route to Haiku", "use a cheaper model", "this is overkill for Opus",
"optimize model usage".
