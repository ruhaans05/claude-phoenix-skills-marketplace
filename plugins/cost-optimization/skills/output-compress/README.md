# output-compress

Makes responses terse and high-signal to cut output tokens — the most expensive tokens you
pay for.

**Why it saves money:** output tokens cost the most per unit. Filler words, pleasantries,
hedging, and restating the question all burn the priciest tokens for no added value.

**What it does:**
- Strips filler, pleasantries, and hedging.
- Prefers tables, bullets, and fragments over paragraphs.
- Leads with the answer instead of previewing it.
- Leaves code, commands, and error strings untouched.

**Guardrail:** stays fully verbose for security warnings, irreversible-action confirmations,
and any multi-step instruction where terseness would create ambiguity.

For a stronger persistent version, see the standalone `caveman` plugin — this is the
lightweight, always-on, token-focused complement.

Trigger phrases: "be brief", "less tokens", "terse", "compress output".
