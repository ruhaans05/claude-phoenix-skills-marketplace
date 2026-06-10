---
name: changelog-keep
description: >
  Maintain a CHANGELOG from commits/PRs in Keep a Changelog format: group changes under
  Added/Changed/Fixed/Removed/Deprecated/Security, write user-facing entries, track an
  Unreleased section. Use when releasing, after a notable change, or when the user says
  "update the changelog", "add a changelog entry", "what changed since last release".
---

# Changelog Keep

A changelog tells users what changed in terms *they* care about — not the commit history.
It's written for the person deciding whether to upgrade, not for the developer who wrote the
code.

## Format (Keep a Changelog + SemVer)

- Top section is `## [Unreleased]` — accumulate entries here as work lands.
- Release sections: `## [1.4.0] - 2026-06-10` (version + ISO date), newest first.
- Group entries under these headings, omit empty ones:
  - **Added** — new features
  - **Changed** — changes to existing behavior
  - **Deprecated** — soon-to-be-removed
  - **Removed** — now-removed
  - **Fixed** — bug fixes
  - **Security** — vulnerability fixes
- On release: rename `[Unreleased]` to the new version + date, start a fresh empty
  `[Unreleased]`.

## Writing entries

- **User-facing language.** "Fixed crash when importing empty CSV" — not "patch null check in
  `parseRow`". The reader doesn't know your internals.
- One line per change, imperative or past tense, consistent within the file.
- Note **breaking changes** prominently (and bump the major version per SemVer).
- Link issues/PRs where the project does.
- Source from merged commits/PRs since the last release, but translate them — a commit is not
  a changelog entry.

## Guardrails

- Only list changes that actually shipped in that version. Don't pre-announce unmerged work
  in a released section.
- Don't dump raw `git log` — filter to user-visible changes and rewrite in user terms. Internal
  refactors with no user impact usually don't belong.
- Keep dates and version numbers accurate and SemVer-consistent — a wrong version/date
  misleads upgraders.
- Security fixes always get a **Security** entry; never hide a vulnerability fix in "Fixed".
