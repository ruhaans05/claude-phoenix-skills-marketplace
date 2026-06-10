---
name: api-docs
description: >
  Document a public API surface so an integrator can use it without reading the source: each
  endpoint/function's purpose, params, return, errors, and a working example. Verify every
  detail against the implementation. Use when documenting a library/HTTP/CLI API, or when the
  user says "document the API", "write API docs", "reference docs".
---

# API Docs

API docs are for someone integrating against your code without reading it. Every detail they
rely on — a param name, a return field, an error code — must be exactly right, because they
build on it. Verify against the implementation, never from assumption.

## Document the public surface only

Cover what's exported/public: HTTP endpoints, exported functions/classes, CLI commands. Skip
internals — documenting private helpers as public API misleads.

## Per entry, document

- **Purpose** — one line: what it does and when to use it.
- **Signature / shape** — method + path for HTTP; signature for functions; usage line for CLI.
- **Parameters / inputs** — name, type, required vs optional, default, constraints, meaning.
  For HTTP: path/query/body params, headers, auth.
- **Returns / response** — type and shape, with field meanings; success status codes for HTTP.
- **Errors** — exceptions raised / error codes / failure responses, and what triggers each.
- **A working example** — real request + real response, or a runnable call. The most-used
  part of any API doc.

## Rules

- **Verify every field against the code.** A wrong param name or return field breaks every
  integrator who trusts it. Read the implementation; don't infer from the name.
- **Examples must run.** Test the example call/request; a broken example is a broken doc.
- Document **stable contract**, and mark anything experimental/deprecated clearly so
  integrators don't build on shifting ground.
- Match the project's existing API-doc format/tool (OpenAPI, JSDoc, docstrings → generated
  reference, etc.).

## Guardrails

- Never document an endpoint/param/return you haven't confirmed exists and behaves as written.
- Keep auth, rate limits, and required headers visible — integrators fail silently without
  them.
- Note breaking changes to a documented API loudly (pairs with `changelog-keep`).
- Don't leak secrets in examples — use placeholder keys, never real credentials (pairs with
  `secret-guard`).
