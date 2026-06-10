---
name: docstring-gen
description: >
  Write accurate docstrings and inline comments that describe what the code actually does, in
  the project's existing convention. Document the contract and the why, not the obvious. Use
  when adding/updating docstrings, commenting a function/class/module, or when the user says
  "add docstrings", "comment this", "document this function".
---

# Docstring Gen

A docstring is a contract: it tells the next person what a thing does, what it expects, and
what it returns — without making them read the body. Wrong or stale docstrings are worse than
none. Accuracy and the project's own convention come first.

## Before writing

1. **Read the implementation.** Document real behavior — actual parameters, real return
   shape, the errors it actually raises, the side effects it actually has. Don't guess from
   the name.
2. **Detect the project's convention.** Look at existing docstrings: Google / NumPy / reST for
   Python; JSDoc/TSDoc for JS/TS; Javadoc, rustdoc, godoc, etc. Match it exactly — format,
   tag style, voice.

## What a good docstring contains

- **One-line summary** of what it does (imperative or descriptive, matching the project).
- **Parameters** — name, type if not in signature, meaning, constraints/units.
- **Returns** — what and in what shape; note `None`/empty cases.
- **Raises / errors** — which exceptions/error values and when.
- **The non-obvious** — side effects, mutation, ordering, thread/async caveats, units, edge
  behavior. This is the highest-value part.

## What to leave out

- Don't restate the signature in prose ("takes x and y and returns the sum") when it adds
  nothing.
- Don't comment self-evident lines (`i += 1  # increment i`).
- Document the **why** for surprising code; the *what* for non-obvious contracts. Obvious
  code needs neither.

## Guardrails

- Never document behavior you haven't confirmed in the code. An invented `@returns` misleads
  every caller.
- Keep the docstring in sync with the signature — if params/returns differ from the doc, the
  doc is the bug. Fix it (or hand to `doc-sync`).
- Match the existing convention even if you'd personally prefer another. Consistency > taste.
- Don't bloat — a precise three-line docstring beats a vague twenty-line one.
