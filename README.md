# 🔥 claude-phoenix-skills-marketplace

A central, world-installable **marketplace of deployable skills for [Claude Code](https://claude.com/claude-code)**.
Organized into **areas** — each area is an installable plugin orchestrated by an agent.

> **First area: Cost Optimization.** Cut the token spend on every agent run. More areas coming.

---

## Install

In Claude Code:

```
/plugin marketplace add ruhaans05/claude-phoenix-skills-marketplace
/plugin install cost-optimization@claude-phoenix-skills-marketplace
```

That's it. The skills and the orchestrator agent become available immediately.

---

## Area: Cost Optimization

Skills + prompts that reduce token usage **during an agent's run** — without sacrificing
quality. Orchestrated by the **`cost-optimizer`** agent, which diagnoses the situation and
points at the right skill.

| Skill | What it does | Saves |
|-------|--------------|-------|
| **model-router** | Routes simple, mechanical subtasks to a cheaper model (Haiku); reserves Opus for hard reasoning. | Model cost per subtask |
| **context-prune** | Drops stale/dead/superseded context so every following turn pays fewer input tokens. | Input tokens, compounding |
| **output-compress** | Terse, high-signal output — tables and fragments over prose. | Output tokens (priciest) |
| **cache-optimizer** | Structures prompts stable-first so Anthropic prompt-cache hits. | Input tokens on reuse |

**Orchestrator — `cost-optimizer` agent:** invoke it (or just say "optimize token usage")
and it tells you the single highest-impact move for your current situation, then names the
skill to apply.

### Quality first

Every skill has a guardrail: never trade a correct answer for saved tokens. The cheap path
is only taken where it doesn't risk the result — security warnings stay verbose, reasoning
tasks stay on the strong model, context you're actively using is never pruned.

---

## How the marketplace is organized

```
claude-phoenix-skills-marketplace/
├── .claude-plugin/marketplace.json     # lists every area/plugin
└── plugins/
    └── cost-optimization/              # one area = one installable plugin
        ├── .claude-plugin/plugin.json
        ├── agents/cost-optimizer.md    # orchestrator for the area
        └── skills/                     # the skills in the area
            ├── model-router/
            ├── context-prune/
            ├── output-compress/
            └── cache-optimizer/
```

Each **area** is a self-contained plugin you can install on its own. Adding a new area =
a new `plugins/<area>/` directory + one entry in `marketplace.json`. See
[CONTRIBUTING.md](CONTRIBUTING.md).

---

## Roadmap

- ✅ **Cost Optimization** — token/cost savings during a run
- 🔜 More areas (testing, refactoring, docs, security, …) — contributions welcome

---

## Privacy & License

- These skills run **locally** inside Claude Code and collect/transmit no data of their own.
  See [PRIVACY.md](PRIVACY.md).
- Licensed under [MIT](LICENSE).
