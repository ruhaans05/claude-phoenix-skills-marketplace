# dep-guard

Vets a dependency before adding or upgrading it — confirming the exact name (no typosquats),
that it's maintained and reputable, the license fits, and the cost is justified.

**Why it helps:** every dependency is code you now trust and ship. Supply-chain attacks
(typosquats, hijacked packages, malicious post-install scripts) and abandoned/bloated
libraries enter through the install command. Vetting first closes that door.

**What it does:**
- Verifies the package name character-for-character against the official source.
- Checks the package is real, maintained, reputable, and actually warranted (vs stdlib/an
  existing dep).
- Confirms license compatibility and pins an exact version; reads the changelog on upgrades.

**Guardrail:** never `--force`s past a dependency conflict or disables lockfile integrity
checks to push an install through; surfaces uncertain packages instead of adding silently.

Trigger phrases: "add a package", "install X", "upgrade dependency", "is this lib safe".
