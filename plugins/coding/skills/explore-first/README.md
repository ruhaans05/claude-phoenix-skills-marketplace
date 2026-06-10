# explore-first

Maps the relevant code and existing patterns **before** you write anything — so Claude reuses
what the project already has instead of duplicating it in a foreign style.

**Why it helps:** the most common agentic-coding mistake is writing code the repo already
has, the wrong way. A few minutes of read-only search up front prevents a wrong-shaped change
and a redo.

**What it does:**
- Turns the task into 2–4 concrete "what do I need to know" questions.
- Searches read-only first (Grep/Glob/Read or parallel Explore subagents).
- Records findings as `file:line` facts, then plans the change to fit the discovered pattern
  and reuse discovered utilities.

**Guardrail:** explores only what the task needs, never edits during exploration, treats
findings as facts to verify.

Trigger phrases: "where does X live", "understand this code", "explore before changing".
