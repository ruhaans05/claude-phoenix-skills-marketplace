---
name: intake
description: >
  City Hall's front desk: parse the user's single build command into the work order that
  drives the whole pipeline — requirements, definition of done, database named or not,
  iterate-until-green or one pass, iteration cap. Use at the start of every Agent City
  run, before any other phase.
---

# Intake

The pipeline gets one prompt from the user. This skill squeezes everything out of it, so
no later phase has to come back asking — extraction first, convention second, and the few
genuinely unanswerable things routed to their sanctioned places.

## Produce the work order

```markdown
## Work Order
- **Build:** <what, in one or two sentences, in the user's own terms>
- **Definition of done:** <observable behavior that proves it works>
- **Stack:** <named in prompt | inferred from repo | chosen by convention — say which>
- **Database:** <named: which | not mentioned: "infer during build" | explicitly none>
- **Iterate:** <until green (default) | one pass — quote the phrase that opted out>
- **Iteration cap:** <from prompt | default 5>
- **Repo/branch context:** <existing repo? default branch? remote?>
- **Out of scope:** <what the prompt implies but does not ask for>
```

## Extraction rules

- **Definition of done is the keystone.** "Build a URL shortener" → done = "POST a URL,
  get a short code; GET the code, get redirected." If the prompt gives acceptance
  criteria, quote them verbatim.
- **Database:** named ("with Postgres", "store users in MongoDB") → record it; the
  archivist consults before construction. Not mentioned → mark "infer during build" —
  the Mayor summons the archivist only if a later phase reports a real persistence need.
  "No database" / "in-memory is fine" → record the opt-out; the archivist stays home.
- **Iteration:** "keep going until it passes" and silence both mean iterate-until-green.
  "Just open the PR", "one pass", "don't loop" mean one pass — quote the exact phrase in
  the work order so the opt-out is auditable.
- **Stack:** prompt's choice wins; otherwise the existing repo's stack; otherwise the
  most conventional choice for the build — record which of the three applied.
- **Out of scope matters.** Listing what you're *not* building is the cheapest scope-creep
  prevention in the pipeline.

## Guardrails

- Don't interrogate the user — ambiguities that convention can settle, convention settles;
  the resolution is disclosed in the work order and the final report.
- Database questions are NOT yours: note that a consult is needed and let the archivist
  batch them at the sanctioned interruption point.
- Never silently expand the request. The work order may sharpen the prompt, not grow it.
