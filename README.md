# 🔥 claude-phoenix-skills-marketplace

A central, world-installable **marketplace of deployable skills for [Claude Code](https://claude.com/claude-code)**,
built around **autonomous ML engineering**. Organized into **collections** — each collection is
an installable plugin orchestrated by an agent.

> **Headline — ML Engineering:** hand the `ml-engineer` agent a prompt or problem statement and
> it frames the task, preps the data, sweeps and tunes models, and selects a winner — looping
> until the goal is met, it's blocked, or it hits an iteration cap you set.
>
> **Collections so far:** ML Engineering (train models autonomously from one prompt), Cost
> Optimization (cut token spend), Coding (agentic coding workflows), Guardrails (safety +
> quality checks), Documentation (write + maintain docs). More coming.

---

## Install

In Claude Code, add the marketplace once, then install the collections you want:

```
/plugin marketplace add ruhaans05/claude-phoenix-skills-marketplace
/plugin install ml-engineering@claude-phoenix-skills-marketplace
/plugin install cost-optimization@claude-phoenix-skills-marketplace
/plugin install coding@claude-phoenix-skills-marketplace
/plugin install guardrails@claude-phoenix-skills-marketplace
/plugin install documentation@claude-phoenix-skills-marketplace
```

The skills and each collection's orchestrator agent become available immediately.

---

## Collection: ML Engineering

The repo's headline. Hand the **`ml-engineer`** agent a prompt or problem statement and it runs
the full ML loop autonomously — writing and running real ML code (pandas / scikit-learn /
XGBoost / LightGBM / PyTorch) — and **keeps iterating until it's done, blocked, plateaus, or
reaches an iteration cap you set in the prompt** (default 5 rounds).

| Skill | Step | What it does |
|-------|------|--------------|
| **problem-framing** | frame | Prompt/problem statement → ML spec: task type, target, metric, data, **definition of done** (built to later accept a Jira ticket) |
| **data-prep** | data | Load, clean, split (leakage-safe), feature-engineer; one split reused every round |
| **model-sweep** | sweep | Baseline + a spread of candidate algorithms, ranked on validation |
| **train-tune** | tune | Hyperparameter search on the top candidates; validation-only |
| **evaluate-select** | evaluate | The single held-out test evaluation + leakage/overfit checks; pick the winner |
| **experiment-loop** | loop | The iteration engine: round counting, error recovery (bounded retries), progress log, continue/stop decision |

**The loop:** frame → prep → sweep → tune → evaluate → decide, repeating. **Stop conditions:**
done (target met + verified), told to stop, blocked (needs something only you can give), cap
reached, or plateau. Error recovery is built in — it reads real tracebacks, fixes the cause,
and retries with a bounded cap rather than dying on the first failure or looping forever.

**Quality first:** the test set is touched exactly once, leakage is actively hunted, metrics
are reported straight, and hitting the cap is never the goal — if it's done in round 2, it
stops at round 2.

```
/plugin install ml-engineering@claude-phoenix-skills-marketplace
```

> Roadmap: the framing step will accept a **Jira ticket** as the problem statement — the agent
> reads the ticket and runs the loop against its acceptance criteria.

---

## Collection: Cost Optimization

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

## Collection: Coding

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

## Collection: Guardrails

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

These compose with the other collections — `ship-it` already leans on several of them; this collection
makes the checks explicit and reusable everywhere. Core stance: default to caution on
anything irreversible or outward-facing, look at the target before destroying it, and report
honestly.

---

## Collection: Documentation

Skills that write and maintain docs as part of the coding loop — accurate to the code,
matched to the audience, kept current. Orchestrated by the **`documentation-orchestrator`**
agent, which maps the doc need to the right skill.

| Skill | For | Output |
|-------|-----|--------|
| **docstring-gen** | Explaining functions/classes inline | Docstrings in the project's convention, describing real behavior |
| **readme-craft** | The project front door | A README a newcomer can act on in minutes |
| **changelog-keep** | Recording what changed | CHANGELOG entries in Keep a Changelog format |
| **api-docs** | A public API surface | Params, returns, errors, working examples per entry |
| **doc-sync** | After a code change | Stale docs found and updated to match new behavior |

The rule under all of them: **document what the code actually does** — verified by reading
it, matched to the audience and the existing style, no more than needed. A confident wrong
doc is the most damaging kind.

---

## How the marketplace is organized

```
claude-phoenix-skills-marketplace/
├── .claude-plugin/marketplace.json     # lists every collection
└── plugins/
    ├── ml-engineering/                 # headline collection
    │   ├── .claude-plugin/plugin.json
    │   ├── agents/ml-engineer.md        # autonomous ML orchestrator
    │   └── skills/
    │       ├── problem-framing/
    │       ├── data-prep/
    │       ├── model-sweep/
    │       ├── train-tune/
    │       ├── evaluate-select/
    │       └── experiment-loop/
    ├── cost-optimization/              # one collection = one installable plugin
    │   ├── .claude-plugin/plugin.json
    │   ├── agents/cost-optimizer.md    # orchestrator for the collection
    │   └── skills/                     # the skills in the collection
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
    ├── guardrails/
    │   ├── .claude-plugin/plugin.json
    │   ├── agents/guardrails-orchestrator.md
    │   └── skills/
    │       ├── no-destruction/
    │       ├── secret-guard/
    │       ├── scope-guard/
    │       ├── dep-guard/
    │       └── verify-before-done/
    └── documentation/
        ├── .claude-plugin/plugin.json
        ├── agents/documentation-orchestrator.md
        └── skills/
            ├── docstring-gen/
            ├── readme-craft/
            ├── changelog-keep/
            ├── api-docs/
            └── doc-sync/
```

Each **collection** is a self-contained plugin you can install on its own. Adding a new
collection = a new `plugins/<collection>/` directory + one entry in `marketplace.json`. See
[CONTRIBUTING.md](CONTRIBUTING.md).

---

## Roadmap

- ✅ **ML Engineering** — autonomous model training from a single prompt (headline)
- 🔜 ML Engineering: **Jira ticket** as the problem-statement input source
- ✅ **Cost Optimization** — token/cost savings during a run
- ✅ **Coding** — agentic coding workflows for the CLI
- ✅ **Guardrails** — safety + quality checks at the risky moments
- ✅ **Documentation** — write + maintain docs that stay true to the code
- 🔜 More collections (testing, performance, …) — contributions welcome

---

## Privacy & License

- These skills run **locally** inside Claude Code and collect/transmit no data of their own.
  See [PRIVACY.md](PRIVACY.md).
- Licensed under [MIT](LICENSE).
