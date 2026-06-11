# 🔥 Phoenix City

**The agent marketplace for [Claude Code](https://claude.com/claude-code).** Home of
**Agent City** — a city of agents that takes one command and returns a GitHub pull
request with passing checks: code written, tested, shipped, and stewarded until green.

> One command in. A passing pull request out. Never merged to main — that click is yours.
>
> Like the phoenix, the pipeline treats every failure as fuel: a red test or a rejected
> review doesn't end the run, it routes back through the city and rises again — until the
> checks burn green or the iteration budget you set runs out.

---

## Install

```
/plugin marketplace add ruhaans05/claude-phoenix-skills-marketplace
/plugin install agent-city@phoenix-city
```

Then give the Mayor a job:

```
build me a URL shortener with Postgres — keep going until the PR passes
```

That's the whole interface. The city handles the rest, and only comes back to you for
database details (credentials, hosting, engine preference) or to confirm anything
destructive.

---

## The City

```
                         ┌─────────────────────┐
                         │       🏛️ MAYOR       │
                         │    (orchestrator)    │
                         │ starts the pipeline, │
                         │  ends the pipeline   │
                         └──────────┬──────────┘
            ┌──────────────┬───────┴───────┬──────────────┐
            ▼              ▼               ▼              ▼
   ┌────────────────┐ ┌─────────────┐ ┌────────────┐ ┌──────────────┐
   │ 🏗️ CITY-ENGINEER│ │🔍 CITY-     │ │📦 CITY-    │ │🗄️ CITY-      │
   │  writes the    │ │  INSPECTOR  │ │  COURIER   │ │  ARCHIVIST   │
   │  code          │ │ writes+runs │ │ opens +    │ │ database —   │
   │                │ │  the tests  │ │ stewards   │ │ only when    │
   │                │ │             │ │  the PR    │ │  needed      │
   └────────────────┘ └─────────────┘ └────────────┘ └──────────────┘
        executive          executive       executive       executive
```

### 🏛️ The Mayor — orchestrator

Takes your one command, runs the pipeline, and is the only one who declares it done.
Starts everything, ends everything, routes every failure back to the right department,
and reports the PR URL with real numbers at the end.

| Skill | What it does |
|-------|--------------|
| **city-charter** | The constitution: phase order, failure routing, iteration bounds, the never-merge-to-main law |
| **intake** | Parses your command into the work order: requirements, definition of done, database status, iterate-or-not, iteration cap |

### 🏗️ City Engineer — writes your code

You prompt it (via the Mayor or directly); it builds. Designs against the existing
codebase first, then implements in small verified increments. Also the repair bay: every
red test and rejected review comes back here with a diagnosis.

| Skill | What it does |
|-------|--------------|
| **blueprint** | Read-only survey → concrete plan: files, interfaces, data flow, reuse targets, build order |
| **construct** | Implements the blueprint increment by increment; repair mode fixes diagnosed causes, not symptoms |

### 🔍 City Inspector — writes and runs your tests

Unit tests, integration tests, and at least one end-to-end test that makes your
definition of done executable. Runs the full suite and triages every failure into a
diagnosis the engineer can act on.

| Skill | What it does |
|-------|--------------|
| **test-forge** | Writes unit + integration tests that would fail on wrong code (each watched failing first) |
| **test-run** | Runs everything relevant, reports real numbers, classifies every red: code bug / bad test / environment |

### 📦 City Courier — ships and stewards your PR

Delivers the work as a pull request — feature branch, honest description, never main —
then manages its life: polls CI, reads failing logs, reads review rejections,
understands *why*, fixes the mechanical, routes the substantive.

| Skill | What it does |
|-------|--------------|
| **pr-open** | Feature branch → clean commits → secret sweep → push → PR with a description traceable to real runs |
| **pr-steward** | Watches checks + reviews; mechanical fixes pushed directly, substantive ones routed back through the pipeline |

### 🗄️ City Archivist — your database, when you need one

Summoned only when the build needs persistence — named in your prompt, or inferred
mid-pipeline. Home of the pipeline's **single sanctioned user interruption**: one batched
exchange for engine choice, credentials, and API keys. Then it provisions and proves it.

| Skill | What it does |
|-------|--------------|
| **db-consult** | Verifies the need is real, recommends the smallest engine that fits, asks you everything in one exchange |
| **db-provision** | Creates/connects, schema as committed migrations, env config + `.env.example`, proven with a real write-and-read-back |

---

## The pipeline

```
your command
  └─► intake          work order: what, done-means, DB?, iterate?, cap
  └─► db-consult      only if DB named or inferred  ← the one user interruption
  └─► blueprint       design against the existing code
  └─► construct       build it
  └─► db-provision    only if consult ran
  └─► test-forge      write the tests
  └─► test-run ──red────► construct (with diagnosis)     ┐
  └─► pr-open         feature branch → push → PR          │  loops until green,
  └─► pr-steward ──red──► construct (with analysis)       ┘  capped (default 5)
  └─► final report    PR URL, real test numbers, check status, what remains
```

**Stop conditions:** PR open and passing (done) · iteration cap reached · plateau (two
loops, no progress) · you opted out of iteration ("just open the PR") · blocked on
something only you can provide. Every stop ends with a truthful report — never a dressed-
up one.

**Immutable laws:** never merge to main · never claim unverified results · never widen
scope past the work order · never bury a failure in a success claim.

---

## Examples

```
# Full autonomous run
build me a REST API for a todo app with auth, keep going until the PR is green

# Database named up front — archivist consults before construction
build a waitlist signup page backed by Supabase

# One pass, no iteration
add rate limiting to the API and just open the PR — don't loop on CI

# Custom iteration budget
build a markdown blog engine, cap it at 3 iterations
```

---

## How the marketplace is organized

```
claude-phoenix-skills-marketplace/
├── .claude-plugin/marketplace.json        # the Phoenix City marketplace
└── plugins/
    └── agent-city/                        # the city, one installable plugin
        ├── .claude-plugin/plugin.json
        ├── agents/
        │   ├── mayor.md                   # orchestrator
        │   ├── city-engineer.md           # executive: code
        │   ├── city-inspector.md          # executive: tests
        │   ├── city-courier.md            # executive: PR/deploy
        │   └── city-archivist.md          # executive: database (conditional)
        └── skills/
            ├── city-charter/   intake/        # mayor
            ├── blueprint/      construct/     # engineer
            ├── test-forge/     test-run/      # inspector
            ├── pr-open/        pr-steward/    # courier
            └── db-consult/     db-provision/  # archivist
```

Each skill ships as `SKILL.md` (the model-facing contract) plus `README.md` (the
human-facing explainer). New districts — more agents, more skills — are welcome: see
[CONTRIBUTING.md](CONTRIBUTING.md).

---

## Roadmap

- ✅ **Agent City v1** — Mayor + four executive agents, one command → passing PR
- 🔜 **City Planner** — a review agent that critiques the blueprint before construction
- 🔜 **Night Watch** — scheduled stewardship: keep tending PRs after the session ends
- 🔜 More districts — observability, performance, security audit. Contributions welcome.

---

## Privacy & License

- Agent City is plain text — Markdown agent definitions, skill instructions, JSON
  manifests. It runs **locally** inside Claude Code and collects nothing.
  See [PRIVACY.md](PRIVACY.md).
- Licensed under [MIT](LICENSE).

---

*Phoenix City — where failed runs rise again.* 🔥
