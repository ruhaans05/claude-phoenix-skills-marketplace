---
name: model-router
description: >
  Route simple, mechanical subtasks to a cheaper, faster model (Claude Haiku) and reserve
  the frontier tier for hard reasoning. Cuts cost without hurting quality. Use when
  delegating subtasks, spawning agents, or when the user says "use a cheaper model",
  "route to Haiku", "this is overkill for the top model", "save model cost", or "optimize
  model usage".
---

# Model Router

Match the model to the task's difficulty. Most subtasks in a run are mechanical and do not
need the most expensive model. Routing them to Haiku cuts cost several-fold with no quality
loss; reserving the frontier tier for genuine reasoning keeps quality where it matters.

The skill is model-agnostic. "Frontier" means whatever the most capable model your run is
on — the routing logic and the savings hold regardless of which specific model that is. The
pricier your frontier model, the bigger the win from pushing mechanical work down a tier.

## Model tiers

| Tier | Example model ID (GA) | Use for |
|------|-----------------------|---------|
| Frontier | `claude-opus-4-8` (or a higher tier, if your run is on one) | Hard reasoning, architecture, ambiguous multi-step tasks, final synthesis |
| Balanced | `claude-sonnet-4-6` | General coding, medium reasoning, most day-to-day work |
| Fast/cheap | `claude-haiku-4-5` | Mechanical, well-specified subtasks (below) |

IDs above are examples of generally-available models — substitute whatever tiers your run
actually has. Cheaper tiers cost a fraction per token AND return faster. Verify current
IDs/pricing with the `/claude-api` skill before quoting numbers.

## Route to Haiku when the subtask is

- Locating code: "where is X defined", "list callers of Y", file/symbol search
- Mechanical edits: renames, typo fixes, format-preserving tweaks, comment removal
- Boilerplate generation from a clear spec
- Summarizing or extracting from a single bounded document
- Classification with a fixed label set
- Anything where the instructions fully determine the output

## Keep on the frontier model when the subtask is

- Open-ended design / architecture
- Multi-file refactors with cross-cutting effects
- Ambiguous requirements needing judgment
- Debugging an unknown root cause
- Final synthesis of many inputs into one answer

## Mechanics

When spawning a subagent via the **Agent** tool, set the `model` parameter to override the
model for that subtask:

```
Agent(subagent_type="general-purpose", model="haiku", description="...", prompt="...")
```

`model` accepts `haiku`, `sonnet`, `opus` (or a full model ID). Omit it to inherit the
parent's model. One frontier-model planning pass that dispatches many Haiku subtasks is the
core cost win.

## Guardrails

- "Frontier" is relative to your run — don't assume a fixed top model; reserve whatever the
  most capable tier you have is for the hardest work.
- Never route a reasoning-heavy task down a tier to save money — a wrong answer costs more
  than the tokens saved.
- If a Haiku subtask returns low-quality output, re-run it one tier up; don't loop on cheap.
- Decide the model BEFORE doing the subtask, not after.
