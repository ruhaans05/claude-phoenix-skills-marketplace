---
name: output-compress
description: >
  Emit high-signal, terse output to cut output tokens — the most expensive tokens per unit.
  Prefer tables, bullets, and fragments over prose; drop filler and hedging. Stay verbose
  only where clarity is safety-critical. Use when responses are long/verbose, the user says
  "be brief", "less tokens", "terse", "compress output", or cost matters.
---

# Output Compress

Output tokens cost the most per unit. Verbose prose, restating the question, hedging, and
filler all burn the priciest tokens for zero added value. Compress without dropping
substance.

## Rules

- Drop filler: "just", "really", "basically", "actually", "simply", "I think".
- Drop pleasantries: "Sure!", "Of course", "Happy to help", "Great question".
- Drop hedging stacks: "it might possibly be the case that" → "likely".
- Don't restate the user's question back to them.
- Don't preview ("Here's what I'll do") then do it — do it.
- Don't summarize what you just said unless the user needs a recap.
- Prefer structure over paragraphs: tables for comparisons, bullets for lists, fragments
  for status. One fact per line.
- Code blocks: leave unchanged. Never compress code, commands, or error strings.
- Lead with the answer; supporting detail after, only if it adds signal.

## Pattern

`[thing] [action/finding] [reason]. [next step].`

Not: "I took a look and it seems like the issue you're running into is probably being caused
by the authentication middleware, where the token expiry check might be using the wrong
comparison operator."

Yes: "Bug: auth middleware token-expiry check uses `<` not `<=`. Fix line 42."

## Stay verbose — do NOT compress

- Security warnings and their consequences
- Irreversible-action confirmations (deletes, force-push, prod changes)
- Multi-step instructions where dropping connective words risks wrong ordering
- Any case where terseness creates genuine ambiguity
- When the user explicitly asked for detail or explanation

Resume compression after the safety-critical part is clear.

## Note

For a stronger, persistent compression mode (full caveman-style prose at configurable
intensity), see the standalone `caveman` plugin. This skill is the lightweight,
always-applicable version focused purely on cutting output-token waste.
