# 🔥 claude-phoenix-skills-marketplace

A central, world-installable **marketplace of deployable skills for [Claude Code](https://claude.com/claude-code)**.
Organized into **areas** — each area is an installable plugin orchestrated by an agent.

> **Areas so far:** Cost Optimization (cut token spend), Coding (agentic coding workflows for
> the CLI), and Guardrails (safety + quality checks). More coming.

---

## Install

In Claude Code, add the marketplace once, then install the areas you want:

```
/plugin marketplace add ruhaans05/claude-phoenix-skills-marketplace
/plugin install cost-optimization@claude-phoenix-skills-marketplace
/plugin install coding@claude-phoenix-skills-marketplace
/plugin install guardrails@claude-phoenix-skills-marketplace
```

The skills and each area's orchestrator agent become available immediately.

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

## Area: Coding

Agentic workflows that make coding with the Claude Code CLI easier — each skill encodes one
proven *loop* instead of ad-hoc editing. Orchestrated by the **`coding-orchestrator`** agent,
which maps your task to the right workflow.

| Skill | What it does | The loop |
|-------|--------------|----------|
| **explore-first** | Maps relevant code + existing patterns before writing, so you reuse instead of duplicate. | Ask → search read-only → record `file:line` facts → plan |
| **tdd-loop** | Drives a feature test-first against real test output. | Red → green → refactor |
| **debug-rca** | Roots out the real cause instead of guess-patching. | Reproduce → isolate → fix cause → verify |
| **safe-refactor** | Restructures without changing behavior, under a test net. | Net → small step → re-run → repeat |
| **ship-it** | Gates the change before it lands. | Review diff → run checks → commit/PR → push |

**Typical feature:** `explore-first` → `tdd-loop` → `ship-it`.
**Typical bug:** `debug-rca` → `tdd-loop` (lock the fix) → `ship-it`.

Every skill shares the same principles: look before you write, verify with the real thing
(run it, don't just read), small reversible steps, match the surrounding code, report
faithfully.

---

## Area: Guardrails

Safety and quality checks that fire at the risky moments of an agentic run — before something
irreversible, leaky, or unverified happens. Orchestrated by the **`guardrails-orchestrator`**
agent, which watches for the risky moment and invokes the matching check.

| Skill | Fires when | Prevents |
|-------|-----------|----------|
| **no-destruction** | About to run a hard-to-reverse command (delete, overwrite, force-push, `DROP`, prod write) | Irreversible damage from an unconfirmed action |
| **secret-guard** | About to commit/log/print anything credential-shaped | Leaking keys, tokens, `.env` values |
| **scope-guard** | Edits drift beyond what was asked | Scope creep — unrelated changes riding along |
| **dep-guard** | About to add/upgrade a dependency | Supply-chain risk: typosquats, unvetted/bloated packages |
| **verify-before-done** | About to say "done" / "fixed" / "works" | False completion claims that weren't run |

These compose with the other areas — `ship-it` already leans on several of them; this area
makes the checks explicit and reusable everywhere. Core stance: default to caution on
anything irreversible or outward-facing, look at the target before destroying it, and report
honestly.

---

## How the marketplace is organized

```
claude-phoenix-skills-marketplace/
├── .claude-plugin/marketplace.json     # lists every area/plugin
└── plugins/
    ├── cost-optimization/              # one area = one installable plugin
    │   ├── .claude-plugin/plugin.json
    │   ├── agents/cost-optimizer.md    # orchestrator for the area
    │   └── skills/                     # the skills in the area
    │       ├── model-router/
    │       ├── context-prune/
    │       ├── output-compress/
    │       └── cache-optimizer/
    ├── coding/
    │   ├── .claude-plugin/plugin.json
    │   ├── agents/coding-orchestrator.md
    │   └── skills/
    │       ├── explore-first/
    │       ├── tdd-loop/
    │       ├── debug-rca/
    │       ├── safe-refactor/
    │       └── ship-it/
    └── guardrails/
        ├── .claude-plugin/plugin.json
        ├── agents/guardrails-orchestrator.md
        └── skills/
            ├── no-destruction/
            ├── secret-guard/
            ├── scope-guard/
            ├── dep-guard/
            └── verify-before-done/
```

Each **area** is a self-contained plugin you can install on its own. Adding a new area =
a new `plugins/<area>/` directory + one entry in `marketplace.json`. See
[CONTRIBUTING.md](CONTRIBUTING.md).

---

## Roadmap

- ✅ **Cost Optimization** — token/cost savings during a run
- ✅ **Coding** — agentic coding workflows for the CLI
- ✅ **Guardrails** — safety + quality checks at the risky moments
- 🔜 More areas (docs, testing, performance, …) — contributions welcome

---

## Privacy & License

- These skills run **locally** inside Claude Code and collect/transmit no data of their own.
  See [PRIVACY.md](PRIVACY.md).
- Licensed under [MIT](LICENSE).
