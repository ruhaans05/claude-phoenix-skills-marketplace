<p align="center">
  <img src="assets/agent-city.svg" alt="Agent City — a skyline of Claude Code agents. The Mayor orchestrates from City Hall; the engineer, inspector, herald, courier, and archivist buildings each carry their skills, while the marshal and the bank patrol the street below. One command in, a passing pull request out." width="100%">
</p>

# Phoenix City

[![License: MIT](https://img.shields.io/badge/license-MIT-orange.svg)](LICENSE)
[![Plugin: agent-city 1.4.0](https://img.shields.io/badge/agent--city-1.4.0-ff5e8a.svg)](plugins/agent-city)
[![Ethics: enforced by the marshal](https://img.shields.io/badge/ethics-enforced%20by%20the%20marshal-ffb347.svg)](ETHICS.md)
[![Built for Claude Code](https://img.shields.io/badge/built%20for-Claude%20Code-7df0c0.svg)](https://claude.com/claude-code)

An agent marketplace for [Claude Code](https://claude.com/claude-code). It ships one
plugin: **Agent City** — a group of agents that work together like a city to take a
single command and turn it into a GitHub pull request with passing checks. Code written,
tested, pushed, and looked after until it's green.

It works where real work happens: your existing repo. The engineer surveys the codebase
before writing anything, new code follows the house conventions, and the inspector runs
your existing suite alongside the tests it writes — a change that breaks old behavior is
a failed inspection, full stop. And it works from the other extreme too: open a
completely empty folder, type `/phoenix build me ...`, and the city founds the repo
itself before it builds.

The pipeline never merges to main. It gets your work to a passing PR; clicking merge is
your job. And the name isn't decoration — when a test fails or a reviewer rejects the PR,
the run doesn't die. The failure gets diagnosed, routed back, and the city rebuilds from
it. That's the phoenix part.

## Install

```
/plugin marketplace add ruhaans05/claude-phoenix-skills-marketplace
/plugin install agent-city@phoenix-city
```

Then give the Mayor a job:

```
/phoenix build me a URL shortener with Postgres — keep going until the PR passes
```

That's the whole interface. (Plain prose works too — "build me X" without the slash
command reaches the Mayor the same way.) Once the pipeline starts, it only comes back to
you for two things: database details (credentials, hosting, engine preference) and
confirmation before anything destructive. Everything else it decides and discloses in
the final report.

Two commands cover the lifecycle:

| Command | What it does |
|---------|--------------|
| `/phoenix <job>` | Start the pipeline — or resume one in flight on the current branch |
| `/city-status` | Read-only: which phase, PR + live check status, what's red, next action |

New to multi-agent systems, or want to understand how this one is put together before
running it? Read [the guide](GUIDE.md) — it teaches how a city of agents works, start to
finish, using this one as the worked example.

### From an empty folder

The pipeline doesn't need a prepared repo. In a directory you created a minute ago:

```
mkdir myapp && cd myapp     # nothing in it, not even git
claude                      # open Claude Code here
/phoenix build me a CLI that tracks my reading list
```

The `groundbreak` skill inspects the ground first: no git → `git init` plus a baseline
commit; no remote → one question (create a private GitHub repo, paste an existing
remote, or run local-only); no `gh` login → it tells you the fix and continues
local-only, so you still end the run with built, tested, committed work on a feature
branch. The ledger remembers — authenticate later, type `/phoenix`, and the run upgrades
itself to a real PR.

The only thing it will never do silently is create a repo on your GitHub account: that
happens once, from your answer, private unless you say public.

## The agents

**The Mayor** is the orchestrator. It parses your command into a work order, runs the
pipeline phase by phase, routes every failure to the right place, and is the only agent
allowed to declare the run done. Its skills: `intake` (command → work order),
`city-charter` (the pipeline's rules — phase order, failure routing, iteration bounds),
and `city-ledger` (the run's record — see below).

The other five are the executive agents. Each owns a phase:

- **city-engineer** writes the code. `blueprint` surveys the existing codebase read-only
  and produces a plan; `construct` builds it in small increments, sanity-checking each
  one. It's also the repair bay — every red test and rejected review lands back here with
  a diagnosis attached.

- **city-inspector** writes and runs the tests. `test-forge` produces unit tests for each
  new behavior, integration tests for the seams, and at least one end-to-end test that
  makes your definition of done executable (every test is watched failing first, so green
  actually means something). `test-run` runs the full suite — new tests plus whatever
  already existed — and triages each failure into a root cause the engineer can act on.

- **city-herald** keeps the docs honest. `doc-sync` runs after the suite is green and
  before the PR opens: it diffs what the run actually built against the README, CHANGELOG,
  usage/help text, and any docs the change touched, then closes the gap — adding what's
  new, correcting what's now wrong, removing what's gone. Because it documents what was
  *verified* (not what was hoped) and its edits ride in the same PR, a reviewer never sees
  code and docs disagree. A pure internal refactor that needs no doc change is allowed to
  be a no-op.

- **city-courier** handles delivery. `groundbreak` makes any directory push-ready
  first — git init and baseline commit in an empty folder, the GitHub question settled
  in one ask, local-only fallback when GitHub isn't reachable. `pr-open` branches off
  the default branch, commits cleanly, sweeps the diff for anything credential-shaped,
  pushes, and opens the PR with a description traceable to real runs. `pr-steward` then
  manages the PR's life: polls
  CI, reads the actual failure logs rather than guessing from check names, reads review
  rejections for their reasoning, fixes mechanical problems itself, and routes
  substantive ones back through the pipeline.

- **city-archivist** is the database agent, and it stays home unless the build needs
  persistence — either you named a database in the prompt, or another agent hit a real
  need for one mid-run. `db-consult` is the pipeline's single sanctioned interruption: it
  batches every question it has (engine, hosting, credentials, API keys) into one
  exchange so it never has to come back. `db-provision` then sets it up — schema as
  committed migrations, app wired through one seam, `.env` gitignored with a committed
  `.env.example` — and proves it with a real write-and-read-back before reporting done.

Two more agents own no phase — they ride *all* of them, read-only:

- **city-marshal** is the police, and it's a department, not a lone officer. The chief
  runs `patrol`'s checkpoints and splits the work across four deputies — **ethics** (is
  the request something the city should build at all?), **licenses & policy** (are the
  licenses clean and attributed?), **secrets & PII** (is anything credential-shaped headed
  for a commit, is personal data collected beyond need?), and **data quality** (do the
  PR's claims match what ran, do migrations replay, are the city's own laws kept?). It's
  the one agent with halt authority, and it can't be routed around — not even by the
  Mayor. Violations stop the run and go to *you*, with evidence. The full stance is in
  [ETHICS.md](ETHICS.md).

- **city-bank** is the treasury — the token-budget watch. It rides every phase read-only
  like the marshal, but with the opposite kind of power: **none over the pipeline**.
  `budget` logs what each phase spends and surfaces optimizations that cost nothing in
  correctness (reuse the ledger instead of re-reading, delegate heavy reads to compressed
  subagents, scope diffs tightly) — but it is purely advisory. It cannot halt, slow, or
  veto a phase, and its first rule is that no suggestion may ever skip a test, starve a
  phase of context it needs, or weaken a marshal check. It makes a run cheaper, never
  worse; the Mayor is free to ignore every word of it.

## The pipeline

```
your command
  └─► intake          work order: what, done-means, DB?, iterate?, cap
  └─► groundbreak     only if the folder isn't a push-ready repo (empty is fine)
  └─► db-consult      only if a DB is named or inferred  ← one merged question batch
  └─► blueprint       design against the existing code
  └─► construct       build it
  └─► db-provision    only if the consult ran
  └─► test-forge      write the tests
  └─► test-run ──red────► construct (with diagnosis)     ┐
  └─► doc-sync        README / CHANGELOG / docs ← what shipped (no-op if nothing user-facing)
  └─► pr-open         feature branch → push → PR          │  loops until green,
  └─► pr-steward ──red──► construct (with analysis)       ┘  capped (default 5)
  └─► final report    PR URL, real test numbers, check status, what remains

  city-marshal patrols every phase (read-only, four deputies, can halt the run)
  city-bank    meters every phase (read-only, advisory — logs cost, never halts)
```

The loop is bounded: five full iterations by default, or whatever cap you set in the
prompt. It also stops early if two consecutive iterations fix nothing — a plateau gets
reported, not ground against. And if you say "just open the PR, don't loop", it does one
pass and reports the PR's true state, red or not.

A few rules hold no matter what:

1. Never merge to main. Green checks don't change this.
2. Never report unverified results — test counts come from real runs, check statuses
   from real polls.
3. Never widen scope past the work order.
4. Never dress up a failure as a success. Caps, plateaus, and blocks end with an honest
   report.

### The ledger — resumable runs, auditable PRs

Every run keeps `.agent-city/ledger.md` on the feature branch: the work order verbatim,
one line per phase transition, every decision made by convention, and an always-current
Status block. Two things fall out of that file:

- **Your session can end mid-run.** Come back tomorrow, type `/phoenix` on the branch, and
  the Mayor verifies the ledger against git and GitHub, then continues from where it
  actually stopped — completed phases are never re-run.
- **Your reviewer gets the full account.** The ledger rides in the PR diff, so whoever
  reviews can see what was asked, what was decided, which tests failed along the way and
  how they were diagnosed. It dies with the branch if the PR is rejected.

Don't want it? Say "no ledger" in the prompt.

## Examples

```
# feature in an existing repo — the common case
/phoenix add CSV export to the reports page, follow the existing download patterns

# full autonomous run, green-field
/phoenix build me a REST API for a todo app with auth, keep going until the PR is green

# database named up front — archivist consults before construction starts
/phoenix build a waitlist signup page backed by Supabase

# one pass, no iteration
/phoenix add rate limiting to the API and just open the PR — don't loop on CI

# custom iteration budget
/phoenix build a markdown blog engine, cap it at 3 iterations

# next morning, on the same branch
/phoenix          # resumes from the ledger
/city-status     # or just ask where things stand
```

## Repo layout

```
.claude-plugin/marketplace.json        # the Phoenix City marketplace
plugins/agent-city/
├── .claude-plugin/plugin.json
├── commands/
│   ├── phoenix.md                     # /phoenix — start or resume the pipeline
│   └── city-status.md                 # /city-status — read-only run report
├── agents/
│   ├── mayor.md                       # orchestrator
│   ├── city-engineer.md               # code
│   ├── city-inspector.md              # tests
│   ├── city-herald.md                 # docs: keeps README + docs in sync
│   ├── city-courier.md                # PR / delivery
│   ├── city-archivist.md              # database (conditional)
│   ├── city-marshal.md                # police: four deputies, rides every phase
│   └── city-bank.md                   # treasury: token budget, advisory, rides every phase
└── skills/
    ├── city-charter/  intake/  city-ledger/      # mayor
    ├── blueprint/     construct/                  # engineer
    ├── test-forge/    test-run/                   # inspector
    ├── doc-sync/                                  # herald
    ├── groundbreak/   pr-open/  pr-steward/       # courier
    ├── db-consult/    db-provision/               # archivist
    ├── patrol/                                    # marshal (ethics · licenses · secrets/PII · data quality)
    └── budget/                                    # bank
```

Each skill is a `SKILL.md` (the model-facing contract) plus a `README.md` (the
human-facing explainer). Want to add a new department to the city, or a new skill to an
existing agent? See [CONTRIBUTING.md](CONTRIBUTING.md).

## Roadmap

- [x] Agent City v1 — Mayor + four executive agents, one command to a passing PR
- [x] v1.1 — `/phoenix` + `/city-status` commands, and the ledger: resumable runs,
      reviewer-auditable PRs ([CHANGELOG](CHANGELOG.md))
- [x] v1.2 — `groundbreak`: start from a completely empty folder; local-only fallback
      when GitHub isn't available
- [x] v1.3 — the marshal: ethics and law enforcement riding every phase, with halt
      authority ([ETHICS.md](ETHICS.md))
- [x] v1.4 — the marshal's four deputies (ethics, licenses/policy, secrets/PII, data
      quality); the **city-herald** (`doc-sync`: README + docs kept in sync with what
      shipped); the **city-bank** (`budget`: advisory token-cost watch that never hinders
      the run) ([CHANGELOG](CHANGELOG.md))
- [ ] City Planner — a review agent that critiques the blueprint before construction
- [ ] Night Watch — scheduled stewardship, tending open PRs after the session ends
- [ ] More districts: observability, performance, security audit

## Privacy, ethics & license

Agent City is plain text — Markdown agent definitions, skill files, JSON manifests. It
runs locally inside Claude Code, makes no network calls of its own, and collects nothing.
Anything the pipeline does (pushing branches, opening PRs, connecting to a database)
happens on your machine with your own credentials. Details in [PRIVACY.md](PRIVACY.md).

The city polices itself: [ETHICS.md](ETHICS.md) covers what it won't build, how the
marshal enforces that on every run, and where the hard lines sit.

MIT licensed — see [LICENSE](LICENSE).
