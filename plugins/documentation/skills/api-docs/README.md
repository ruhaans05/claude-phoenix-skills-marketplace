# api-docs

Documents a public API surface so an integrator can use it without reading the source —
purpose, params, return, errors, and a working example for each entry point.

**Why it helps:** integrators build on every detail of your API docs. A wrong param name or
return field breaks everyone who trusts it. This skill verifies every detail against the
implementation.

**What it does:**
- Documents only the public surface (endpoints, exported functions, CLI commands) — not
  internals.
- Captures purpose, signature, inputs, returns, errors, and a runnable example per entry.
- Matches the project's existing API-doc format (OpenAPI, JSDoc, generated reference, …).

**Guardrail:** never documents an unconfirmed endpoint/param/return, requires examples that
actually run, keeps auth/rate-limit info visible, and uses placeholder keys (never real
credentials) in examples.

Trigger phrases: "document the API", "write API docs", "reference docs".
