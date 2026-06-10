# docstring-gen

Writes accurate docstrings and inline comments that describe what the code **actually does**,
in the project's existing convention — documenting the contract and the *why*, not the
obvious.

**Why it helps:** a docstring is a contract that saves the next person from reading the body.
Wrong or stale docstrings are worse than none. This skill reads the implementation first and
matches the project's docstring style.

**What it does:**
- Reads the implementation to capture real params, return shape, errors, and side effects.
- Detects and matches the project's convention (Google/NumPy/reST, JSDoc/TSDoc, rustdoc, …).
- Emphasizes the non-obvious (side effects, units, ordering, caveats) and skips restating the
  signature.

**Guardrail:** never documents unconfirmed behavior, keeps the doc in sync with the
signature, matches the existing convention over personal taste, and stays concise.

Trigger phrases: "add docstrings", "comment this", "document this function".
