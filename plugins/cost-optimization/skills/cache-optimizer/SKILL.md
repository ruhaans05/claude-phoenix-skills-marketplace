---
name: cache-optimizer
description: >
  Structure prompts and tool order to maximize Anthropic prompt-cache hits, so repeated or
  long-prefix calls re-read cached tokens at a fraction of the price. Put stable content
  first, volatile content last; keep tool/system order steady. Use when making repeated
  model calls, working with a long stable prefix, or the user says "optimize caching",
  "improve cache hits", "reduce input cost", "prompt caching".
---

# Cache Optimizer

Anthropic prompt caching lets a stable prompt prefix be re-read at a large discount instead
of full price on each call. The win is mechanical: order content so the unchanging part sits
in front and gets cached; keep the cached prefix byte-identical across calls.

## How the cache works (essentials)

- Caching matches on an exact prefix. The longest identical leading span of the request hits
  the cache; everything from the first byte of difference onward is full-price.
- Cache entries have a short TTL (about 5 minutes by default; refreshed on each hit). Calls
  spaced beyond the TTL re-pay to warm the cache.
- Cached input tokens cost a fraction of normal input tokens; writing to the cache costs a
  small premium once. So caching pays off when a prefix is reused at least twice within TTL.
- Verify current TTL, pricing multipliers, and the exact API mechanism (`cache_control`
  breakpoints) with the `/claude-api` skill before quoting numbers.

## Structure prompts: stable first, volatile last

Order request content from least-likely-to-change to most-likely-to-change:

1. System prompt / role instructions (stable)
2. Tool definitions (stable)
3. Large reference material — big files, schemas, docs (stable across the task)
4. Conversation history (grows, but earlier turns are stable)
5. The current user turn / volatile inputs (always last)

A single changed byte early in the prompt invalidates the cache for everything after it. So
never interleave a volatile value (timestamp, random ID, per-call counter) into the stable
prefix.

## Keep the prefix identical across calls

- Don't reorder tool definitions between calls — same set, same order.
- Don't inject per-call noise (timestamps, UUIDs, "attempt N") into the system prompt.
- Pin the model and system prompt for a batch of related calls.
- When looping over many items, hold the shared instructions/context as a fixed prefix and
  vary only the trailing per-item content.

## When it pays off

- Repeated calls sharing a long prefix (batch classification, multi-item extraction, agent
  loops re-sending the same system + tools).
- One big stable document queried many times.
- Tight loops within the 5-minute TTL.

## When it doesn't

- One-shot calls with no reuse — caching adds a small write premium for no later hit.
- Prefixes that change every call — nothing stable to cache.
- Calls spaced far beyond the TTL — the entry expires between uses.

## Guardrail

Don't contort prompt content into a worse structure purely to chase cache hits. Correct,
clear prompts first; cache-friendly ordering second, where it's free to apply.
