---
name: dep-guard
description: >
  Vet a dependency before adding or upgrading it: confirm the exact name (no typosquats),
  that it's maintained and reputable, the license fits, and the cost is justified. Guards the
  supply chain. Use before npm/pip/cargo/go installs, before bumping versions, or when the
  user says "add a package", "install X", "upgrade dependency", "is this lib safe".
---

# Dependency Guard

Every dependency is code you now trust and ship. Supply-chain attacks (typosquats, hijacked
packages, malicious post-install scripts) and abandoned/bloated libraries enter through the
install command. Vet before you add.

## Before adding or upgrading

1. **Verify the exact name.** Typosquatting is the #1 supply-chain vector — `lodahs`,
   `colour`/`colors`, `python-requets`. Match it character-for-character against the official
   package page, not memory.
2. **Check it's real and maintained.** Recent releases, meaningful download counts, an active
   repo, resolved issues. A brand-new package with no history pulling a popular name is a red
   flag.
3. **Is it warranted?** Don't add a dependency for something the stdlib or an existing project
   dep already does (find it via `explore-first`). A one-function utility rarely justifies a
   new package and its transitive tree.
4. **License check.** Confirm the license is compatible with the project (watch GPL/AGPL in
   permissive-licensed code).
5. **Pin and review the bump.** Add an exact/locked version. For an upgrade, read the
   changelog for breaking changes and check it's not a major jump done blindly.

## Red flags — stop and surface

- Name one character off from a popular package.
- Install triggers a post-install script you can't account for.
- Maintainer/owner changed recently on a previously trusted package.
- Huge transitive dependency tree for a small need.
- Unmaintained (no release in years) for security-sensitive functionality.

## Guardrails

- Never `--force`/`--legacy-peer-deps` past a dependency conflict just to make install
  succeed — understand the conflict first.
- Never disable lockfile integrity checks (`--no-verify`, ignoring `package-lock`/`poetry.lock`
  hashes) to push an install through.
- Don't auto-update everything to "latest" in bulk; upgrade deliberately, one concern at a
  time, with the changelog read.
- When a dependency's safety is uncertain, surface it and let the user decide — don't add it
  silently.
