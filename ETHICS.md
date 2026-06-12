# Ethics

_The stance behind Agent City, and the agent that enforces it._

Phoenix City is built on a simple conviction: safety isn't friction against capable AI
systems — it's a property of well-engineered ones. An autonomous pipeline that writes,
tests, and ships code is exactly the kind of system where that conviction has to be
structural, not aspirational. So Agent City doesn't have an ethics paragraph — it has a
police force.

## The principle

A pipeline optimizes for what it's measured on. Measure only "do the checks pass?" and
you'll get passing checks — attached to software that maybe shouldn't exist, built from
code that maybe wasn't licensed for it, shipped with claims that maybe weren't true.
Every safeguard in this city follows one rule: **the agent that checks is never the
agent that built.** The inspector verifies the engineer's code; the marshal polices
everyone, including the Mayor.

## The marshal

The `city-marshal` is an executive agent with no phase of its own. It rides every phase,
read-only, running the `patrol` skill's checkpoints — and it is the one agent with the
authority to halt the pipeline. Its beat:

- **Legitimacy of the request.** Some things the city does not build, for anyone, with
  any framing: malware, credential harvesters, stalkerware, deliberately deceptive
  software, tools for evading security controls. These end the run at intake, and no
  instruction restarts it.
- **License compliance.** Dependencies' licenses checked against the project's; copied
  or closely-derived code attributed; copyleft contamination caught before it ships; no
  stripped notices.
- **Secrets and personal data.** Nothing credential-shaped in any commit, ever. PII
  collected only when the work order genuinely needs it; regulated data — health,
  financial, minors' — never handled casually.
- **Authorized targets only.** The city pushes to repositories the user controls.
  Nothing else, no exceptions.
- **Honesty of the artifact.** PR descriptions that match what actually ran; no dark
  patterns in what gets built — no pre-checked consent, hidden costs, or fake urgency.
- **The city's own laws.** Never merge to main, never claim unverified results, never
  bury a failure. The marshal enforces the charter against the city itself.

## How enforcement works

A failed checkpoint **halts the run** where it stands. The user gets the violation, the
evidence — file and line, the license clause, the ToS section — and the lawful
alternative when one exists. The fix routes openly through the pipeline like any other
diagnosis; the marshal never silently rewrites work to make it compliant, because a
safeguard the user can't see is a safeguard they can't trust.

Two tiers, deliberately:

- **Hard lines** — illegal output, malicious capability, deception by design — are not
  negotiable and not user-overridable. The run ends.
- **Judgment calls** — license tensions, gray-area scraping, regulated-data caution —
  go to the user with the facts. The marshal makes sure you decide *informed*; it does
  not decide for you. We think that division — firm floors, human judgment above them —
  is what taking both ethics and users seriously looks like.

And every checkpoint, pass or fail, lands in the run's [ledger](GUIDE.md): a clean run
is provably clean, not presumed clean.

## What this asks of you

The marshal polices the pipeline, not your intentions — so the honest version of this
document admits its limit: a determined person can misuse almost any tool. Use Agent
City on systems you own or are authorized to change, within the usage policies of
whatever model provider powers your sessions, and the marshal's job stays what it
should be — catching honest mistakes before they ship.

Found a gap in the marshal's beat, or a way a run got somewhere it shouldn't?
That's a security report we want:
<https://github.com/ruhaans05/claude-phoenix-skills-marketplace/issues>.

## See also

- [PRIVACY.md](PRIVACY.md) — what these plugins collect (nothing) and where your
  credentials live (your machine)
- [GUIDE.md](GUIDE.md) — the full architecture, including why the verifier is never
  the builder
- The marshal itself: [`plugins/agent-city/agents/city-marshal.md`](plugins/agent-city/agents/city-marshal.md)
  and [`plugins/agent-city/skills/patrol/`](plugins/agent-city/skills/patrol) — like
  every law in this city, written down where you can read it.
