---
name: readme-craft
description: >
  Write or update a README that a newcomer can act on in minutes: what it is, why it exists,
  install, minimal working usage, then deeper detail. Lead with value, keep commands correct.
  Use when creating/updating a README, or when the user says "write a README", "improve the
  readme", "document the project".
---

# README Craft

The README is the product's front door. A newcomer decides in the first screen whether to
keep going. Optimize for that reader: lead with what it does and why they'd want it, then get
them running fast, then go deep.

## Structure (top to bottom = most to least important)

1. **Name + one-line what-it-is.** A reader knows in one sentence whether this is for them.
2. **Why / the value.** The problem it solves, the before/after. Keep it concrete.
3. **Install.** The exact commands, copy-pasteable, accurate. One broken install command
   loses a real user.
4. **Quickstart / minimal usage.** The smallest example that does something real. Show input
   and output. This is the most-read section — make it work end to end.
5. **Deeper usage / configuration / API** — for readers who stayed.
6. **Links** — contributing, license, docs site, support.

## Writing rules

- **Lead with the answer, not history.** No long preamble before the reader learns what it
  is.
- **Show, don't just tell.** A working code/CLI example beats a paragraph describing it.
- **Every command must actually run.** Test install/quickstart commands; a typo here is the
  most expensive doc bug.
- **Right for the audience.** A README is for users/newcomers, not maintainers — explain
  jargon or link it; don't assume internals knowledge.
- **Scannable** — headings, short paragraphs, tables for options. People skim READMEs.
- **Keep tables/feature lists synced with reality.** A feature listed but removed (or shipped
  but unlisted) erodes trust.

## Guardrails

- Never document install steps or features you haven't verified exist and work. An aspirational
  README is a broken one.
- Don't bury the value under badges, TOC, and boilerplate — the pitch comes first.
- Updating an existing README: preserve its voice and structure; change what's wrong, don't
  rewrite wholesale (pairs with `scope-guard`).
- Keep version-specific commands current; stale version numbers in install commands break for
  new users.
