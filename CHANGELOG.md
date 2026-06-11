# Changelog

All notable changes to the Phoenix City marketplace and the Agent City plugin.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.1.0] — 2026-06-11

### Added
- `/city` slash command — deterministic entry point for the pipeline; detects and
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
