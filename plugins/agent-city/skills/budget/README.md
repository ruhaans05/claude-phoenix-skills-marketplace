# budget

The city-bank's continuous, advisory-only watch on token spend. It rides every phase
read-only, logs what each one costs, and — only when there's a real saving that costs
nothing in correctness — tells the Mayor how the same result could be reached for less.

**Why it helps:** a long autonomous run can burn tokens on work it already did —
re-reading files the ledger already summarized, re-deriving the blueprint, dragging whole
files into context for a two-line change. The bank is the meter that notices, so a run
stays affordable without anyone watching the gauge by hand.

**Why it can't hurt:** unlike the marshal, the bank has **no halt authority and no veto**.
It cannot stop, pause, or slow a phase; it only advises, and the Mayor is free to ignore
it. That's deliberate — a cost watchdog that could block would eventually block good
engineering to save money. The first rule of the budget is that no suggestion may ever
skip a test, starve a phase of context it needs, weaken a marshal check, or lower the
quality bar. Every win is "same work, less waste," never "less work."

**Where the savings come from (all correctness-free):**
- reuse the ledger and blueprint instead of re-reading the codebase
- delegate heavy locate/survey reads to compressed subagents, consume the summary
- read the spans a phase needs, not whole files
- scope to what changed; drop duplicate tool calls

**Guardrail:** advisory only — no halt, no slowing. Estimates are labeled as estimates,
never invented. Safety and correctness outrank the budget every time they meet. A lean
phase gets one quiet ledger line and no nagging.

Trigger phrases: rides every phase automatically; also "how many tokens is this costing",
"optimize token usage", "reduce the context", "why is this run expensive".
