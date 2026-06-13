# Changelog

All notable changes to the Phoenix City marketplace and the Agent City plugin.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.4.0] — 2026-06-13

### Added
- `city-herald` agent + `doc-sync` skill — a documentation phase between a green
  `test-run` and `pr-open`. Brings README, CHANGELOG, usage/help text, and touched docs
  into truth with what the run actually built, so docs ride in the same PR as the code.
  Documents what was *verified*, not what was hoped; a pure internal change may be a no-op.
- `city-bank` agent + `budget` skill — the treasury. Rides every phase read-only and
  **advisory only**: logs per-phase token spend to the ledger and surfaces correctness-free
  optimizations (reuse the ledger/blueprint, delegate heavy reads to compressed subagents,
  scope diffs tightly). No halt, no veto, no slowing — its first rule is that no suggestion
  may skip a test, starve a phase of needed context, or weaken a marshal check.
- The marshal's four deputies — `patrol` now splits its sweep across **ethics** (should
  this exist?), **licenses & policy** (compliance, attribution, ToS), **secrets & PII**
  (credentials, personal data), and **data quality** (replayable state, honest claims, the
  city's own laws). One sweep, four beats; every flag names its deputy.

### Changed
- `city-marshal`: rewritten as chief + deputy roster; halt authority and read-only mandate
  unchanged. The deputy of ethics owns the hard lines; data quality owns the city's laws.
- Mayor: pipeline gains the `doc-sync` phase; pipeline table adds the herald (Document)
  and the bank (Watch the cost — advisory, never hinders), and notes both the marshal and
  bank ride every phase.
- README, repo layout, roadmap, and the skyline visual updated for the herald, the bank,
  and the deputies.

## [1.3.0] — 2026-06-11

### Added
- `city-marshal` agent — Agent City's police. Owns no phase; rides every phase
  read-only with halt authority no agent (the Mayor included) can route around.
- `patrol` skill — checkpoint sweeps at every phase transition: request legitimacy,
  license compliance, secrets/PII, authorized targets, honest claims, and the charter
  itself. Hard violations end the run; judgment calls go to the user with evidence.
- [ETHICS.md](ETHICS.md) — the stance: safety as a property of well-engineered
  systems, firm floors with human judgment above them, every checkpoint logged to the
  ledger so a clean run is provably clean.

### Changed
- Charter: new Article VI (the marshal); immutable laws renumbered to Article VII.
- Mayor: marshal row in the pipeline table — a marshal halt is never routed around.

## [1.2.0] — 2026-06-11

### Added
- `groundbreak` skill (city-courier) — the pipeline now starts from a completely empty
  folder: detects missing git / commits / remote / gh auth, founds the repo (git init +
  baseline commit), settles the GitHub question in one ask (private repo by default,
  existing remote, or local-only), and degrades gracefully to a local-only run that
  ends at a finished feature branch instead of a PR.
- Local-only mode as a sanctioned terminal state; `/phoenix` later upgrades a
  local-only run to a real PR via the ledger.

### Changed
- Charter Article IV: three sanctioned interruptions (groundbreak ask, archivist
  consult, safety) — merged into one batch when both questions are pending.
- `intake` work order: repo-readiness, visibility, and local-only fields.
- `pr-open`: a missing remote routes back through groundbreak instead of failing.
- `/phoenix` command: groundbreak step added; "an empty folder is a valid starting
  point."

## [1.1.0] — 2026-06-11

### Added
- `/phoenix` slash command — deterministic entry point for the pipeline; detects and
  resumes a run in flight.
- `/city-status` slash command — read-only report: ledger, branch, PR, live check
  status, next action.
- `city-ledger` skill — `.agent-city/ledger.md` on the feature branch: work order,
  phase log, decisions made by convention, always-current Status block. Runs are now
  resumable across sessions and auditable by the PR reviewer. Opt out with "no ledger".
- This changelog.

### Changed
- Mayor: resume-before-restart rule; keeps the ledger at every phase transition.
- `city-charter`: new Article V (the public record); laws renumbered to Article VI.
- `intake`: opens the ledger after producing the work order; hands resumes back to the
  Mayor.

## [1.0.0] — 2026-06-11

### Added
- Agent City: the Mayor (orchestrator) + four executive agents — city-engineer,
  city-inspector, city-courier, city-archivist.
- Ten skills: `city-charter`, `intake`, `blueprint`, `construct`, `test-forge`,
  `test-run`, `pr-open`, `pr-steward`, `db-consult`, `db-provision`.
- The pipeline: one command → work order → code → tests → pull request, looping on
  failures (default cap 5, plateau detection) until checks pass. Never merges to main.
- README with the animated skyline, [GUIDE.md](GUIDE.md), [PRIVACY.md](PRIVACY.md),
  [CONTRIBUTING.md](CONTRIBUTING.md).

### Removed
- The previous skills collections (ml-engineering, cost-optimization, coding,
  guardrails, documentation) — preserved in git history before `b922666`.
