# cache-optimizer

Structures prompts and tool order so Anthropic's prompt cache gets hit, letting repeated or
long-prefix calls re-read cached tokens at a fraction of the normal input price.

**Why it saves money:** cached input tokens cost a fraction of full-price ones. If a long
stable prefix (system prompt, big file, tool defs) is reused across calls, caching it turns
the most repetitive part of your spend into a discount.

**What it does:**
- Orders content stable-first, volatile-last so the cacheable prefix is as long as possible.
- Keeps the prefix byte-identical across calls (no timestamps/UUIDs in the system prompt, no
  reordering tool defs).
- Explains when caching pays off (repeated calls within the ~5-min TTL) and when it doesn't.

**Guardrail:** never contorts a prompt into a worse structure just to chase cache hits —
correctness and clarity come first.

Trigger phrases: "optimize caching", "improve cache hits", "reduce input cost", "prompt
caching".
