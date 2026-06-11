# How a city of agents works

A guide to multi-agent systems for people who'd rather understand one real example than
read a survey paper. The example here is [Agent City](README.md), but the ideas apply to
any agent marketplace you'll evaluate — by the end you should be able to look at one and
know within five minutes whether it's well-built.

## 1. The problem with one very smart agent

Give a single agent a job like "build me a todo API with auth and get it to a passing
PR" and it will usually do something impressive — and then quietly fall apart in one of
three places.

First, **context**. One agent doing everything carries everything: the codebase survey,
the failed test output from twenty minutes ago, the half-finished migration, the CI log.
Long sessions degrade. The agent starts forgetting what it decided earlier, or worse,
re-deciding it differently.

Second, **conflict of interest**. The agent that wrote the code is now asked whether the
code works. It's not lying when it says yes — it's pattern-matching its own intent
instead of running the thing. Every "done!" that wasn't, came from this.

Third, **scope**. Without a boundary, an agent fixing a bug will also refactor the
module, rename three things, and upgrade a dependency. Each step locally reasonable;
the diff unreviewable.

Cities solved all three problems centuries ago, which is why the metaphor isn't just
branding. A city is what division of labor looks like when it works: specialists with
jurisdictions, an administration that routes work between them, and laws that bind
everyone — including the administration.

## 2. The two building blocks: agents and skills

Every well-built agent system separates **who** from **how**.

An **agent** is a who: a role with a jurisdiction, its own working context, and operating
rules. In Agent City the engineer writes code, the inspector tests it, the courier ships
it, the archivist handles the database. Crucially, the inspector is not the engineer —
the one who verifies is never the one who built. That's the conflict-of-interest fix,
structurally, not as a promise.

A **skill** is a how: a written procedure for one phase of work. The contract that says
exactly what the phase consumes, what it produces, and what it must never do. Agent
City's engineer runs two: `blueprint` (survey the codebase read-only, produce a plan)
and `construct` (build the plan in small verified increments). The agent brings
judgment; the skill brings discipline. You want both, and you want them separate —
because a skill you can read is a behavior you can audit. When a marketplace shows you
its skills as plain files, you can check what the system will refuse to do *before*
running it. When it doesn't, you're trusting vibes.

A good test when you're reading any marketplace: open one skill file. If it says what
the phase produces and what it must never do, the authors were thinking about contracts.
If it's a paragraph of enthusiasm, keep shopping. (Agent City's are in
[`plugins/agent-city/skills/`](plugins/agent-city/skills) — each one is a `SKILL.md`
contract plus a human-readable `README.md`. Open `pr-open` and look for the word
"never". You'll find it doing real work.)

## 3. Someone has to be the mayor

Specialists alone aren't a system — four brilliant agents with no coordinator just
produce four opinions. Something has to own the pipeline: start it, sequence it, route
failures, enforce bounds, and be the *single* place that declares the job done.

That's the orchestrator. In Agent City it's literally called the **Mayor**, and its two
skills are the whole administration:

- `intake` turns your one command into a **work order** — what to build, what "done"
  observably means, whether a database is involved, whether to iterate until green, and
  the iteration cap. Everything decidable up front gets decided up front, which is the
  only way "one command in" can be honest.
- `city-charter` is the constitution: phase order, failure routing, bounds, and the laws
  no agent may break.

Two orchestrator details separate serious systems from demos:

**Failures route; they don't end the run.** When Agent City's inspector finds a red
test, it doesn't report "failed" — it produces a diagnosis ("session token issued before
the password check, `auth.py:42`") and the Mayor routes it back to the engineer's repair
bay. Same for a rejected PR review: the courier extracts the reviewer's *reasoning*,
restates it, and routes it. A failure that comes back as a diagnosis is progress; a
failure that comes back as "failed" is a stalled run.

**The loop is bounded.** "Iterate until it passes" without a cap is a money printer for
your API bill. Agent City defaults to five full iterations (you can set your own in the
prompt), and stops early on a *plateau* — two consecutive loops that fix nothing. Either
way it stops by telling you exactly what's still red and why, not by claiming victory.

## 4. Autonomy is a budget — spend interruptions like money

The whole point of a pipeline is that you type one command and walk away. Every time the
system comes back with a question, it spends that. So a well-designed system decides, in
advance, exactly when it's allowed to interrupt you — and batches ruthlessly.

Agent City permits exactly two interruptions:

1. **The archivist's consult.** Databases genuinely need you: engine preference,
   credentials, API keys for hosted providers. So the `db-consult` skill asks once, asks
   *everything* in a single exchange, and lists a safe default next to each question
   where one exists. A second database question later in the run is, by the system's own
   charter, a failure of this skill. And the archivist only shows up at all if the build
   needs persistence — named in your prompt, or inferred mid-run from a real need. No
   database gets bolted on because the system likes databases.
2. **Safety.** Confirmation before anything destructive or irreversible, always.
   Autonomy never covers dropping a table.

Everything else gets decided by convention and *disclosed in the final report* rather
than asked about. That last clause matters: silent decisions are how systems surprise
you. Disclosed decisions are how they earn trust.

## 5. The laws — what the system refuses to do

Capability is what a system can do; character is what it refuses to do. When you
evaluate any agent marketplace, the refusals are the part to read first. Agent City's
charter has four, and every agent is bound by them:

1. **Never merge to main.** The pipeline's terminal state is an open, passing pull
   request. Merging is a human decision made on the PR page — green checks don't change
   that, and neither does anything else. A coding pipeline that merges its own work has
   removed the one review point that matters.
2. **Never claim unverified results.** Test counts come from real runs, check statuses
   from real polls. The inspector reports the numbers the suite actually printed —
   and every test it writes is watched *failing first*, because a test that can't fail
   verifies nothing.
3. **Never widen scope.** The work order is the boundary. The intake skill even records
   what's *out* of scope, because listing what you're not building is the cheapest
   scope-creep prevention there is.
4. **Never bury a failure.** Cap reached, plateau hit, blocked on a credential — the run
   ends with a truthful report of what's red and why. A one-pass PR with failing tests
   opens with the failures at the top of its own description.

Plus one that runs underneath all four: **secrets never touch the repo**. Credentials
live in a gitignored `.env`; the committed file is `.env.example` with placeholders; and
the courier sweeps the diff for anything credential-shaped before every push, because
the last cheap place to catch a leaked key is before it's published.

## 6. A full run, start to finish

Theory done. Here's what actually happens when you type:

```
build me a URL shortener with Postgres — keep going until the PR passes
```

**Intake.** The Mayor parses the command into a work order. Build: URL shortener.
Done means: POST a URL, get a short code; GET the code, get redirected. Database:
Postgres — named, so the archivist is summoned before construction. Iterate: until
green. Cap: 5 (default, nothing else specified).

**Consult.** The archivist confirms Postgres and asks its one batched question set:
local Postgres or hosted? If hosted — connection string? (Local listed as the default if
you don't care.) You answer once. That's the last you'll hear from the pipeline unless
something destructive needs a yes.

**Blueprint.** The engineer surveys the repo read-only — layout, conventions, anything
reusable — and produces the plan: data model, the two endpoints, where config lives,
build order in small increments, persistence flagged as "uses provisioned DB".

**Construct.** The engineer builds increment by increment — model, then logic, then
HTTP layer — sanity-checking each piece (does it import? does it run?) before stacking
the next on top.

**Provision.** The archivist creates the database, writes the schema as committed
migration files, wires the app's data layer through one seam, sets up `.env` /
`.env.example` — then proves the whole thing with a real write-and-read-back through the
app's own code. "The config looks right" doesn't count as provisioned here.

**Forge and run.** The inspector writes unit tests for each behavior, integration tests
for the route→logic→database seams, and one end-to-end test that *is* the definition of
done, executable. Then runs everything. Say a redirect test comes back red: the
inspector triages it to a root cause — trailing-slash handling in the route, with the
file and line — and the Mayor routes that diagnosis back to construct. Fix, re-run,
green. That was iteration one of five, and it cost you nothing but tokens.

**Open.** The courier branches (`agent-city/url-shortener`), commits cleanly, sweeps
the diff for secrets, pushes, and opens the PR — description carrying what was built,
the inspector's real numbers, the database setup, and any conventions it chose on your
behalf.

**Steward.** CI runs. A check fails that passed locally — the courier pulls the actual
log, not the check name, and diffs the environments: CI's Postgres is version-pinned
differently. Mechanical fix, pushed directly. Checks go green. If a human reviewer later
requests changes, the same machinery handles it: extract the reasoning, route
substantive work back through engineer and inspector, respond on the PR thread with
what changed and in which commit.

**Report.** The Mayor ends the run: PR URL, branch, what was built, real test counts,
check status, database details, iterations used (two of five), decisions made on your
behalf. The merge button is yours.

One more thing rode along the whole way: the **ledger** (`.agent-city/ledger.md`,
committed on the feature branch). Every phase transition, every routed failure, every
convention-decision — one line each, plus a Status block that's always current. It's why
the run survives your session ending (type `/phoenix` on the branch tomorrow and the Mayor
resumes from the verified phase, never re-running completed work) and why your reviewer
gets the full account of how the PR was made, right in the diff. Most agent systems
treat the run as ephemeral; treating it as a *record* is what makes autonomy something a
team can actually adopt.

## 7. How to judge any agent marketplace (including this one)

Take this checklist to anything you're evaluating. It's the distilled version of
everything above:

- [ ] **Separate builder and verifier?** If the same agent writes and approves the code,
      "done" means "I meant to."
- [ ] **Skills readable as contracts?** Plain files stating inputs, outputs, and
      refusals — auditable before you run anything.
- [ ] **One owner of start and end?** An orchestrator that declares done exactly once,
      with evidence.
- [ ] **Failures become diagnoses?** Routed back with root causes, not "failed".
- [ ] **Bounded loops?** A default cap, user-overridable, with plateau detection.
- [ ] **Explicit interruption policy?** Knows when it may ask you things, and batches.
- [ ] **Refusals in writing?** Won't merge to main, won't commit secrets, won't claim
      unverified, won't act destructively unconfirmed.
- [ ] **Honest endings?** Stops by telling you what's red, never by dressing it up.

Agent City checks all eight — not because this guide says so, but because every claim
above maps to a file in this repo you can open and read. That's the real pitch: not
that the system is impressive, but that it's *inspectable*.

## Try it

```
/plugin marketplace add ruhaans05/claude-phoenix-skills-marketplace
/plugin install agent-city@phoenix-city
```

Then:

```
build me a tiny CLI that reverses a string, one pass
```

Thirty seconds of city in motion, and you'll see every concept in this guide go by:
intake, blueprint, construct, forge, run, open, report. Then give it something real.
