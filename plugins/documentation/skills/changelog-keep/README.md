# changelog-keep

Maintains a CHANGELOG from commits/PRs in Keep a Changelog format — grouping entries under
Added/Changed/Fixed/Removed/Deprecated/Security and tracking an Unreleased section.

**Why it helps:** a changelog tells users what changed in terms *they* care about, for the
person deciding whether to upgrade — not a raw dump of commit history.

**What it does:**
- Accumulates entries under `## [Unreleased]`, then cuts a dated, SemVer-versioned release
  section.
- Writes user-facing entries (translates commits into user terms) and flags breaking changes.
- Always gives security fixes a dedicated **Security** entry.

**Guardrail:** only lists what actually shipped in a version, never dumps raw `git log`, keeps
dates and version numbers SemVer-consistent.

Trigger phrases: "update the changelog", "add a changelog entry", "what changed since last
release".
