# problem-framing

Turns a prompt or problem statement into a concrete ML spec — task type, target, evaluation
metric, data, constraints, and an explicit definition of done — before any code is written.

**Why it helps:** most ML failures are framing failures (wrong metric, ambiguous target, no
clear bar for "done"). Fixing the spec first means everything downstream is judged against
the right objective.

**What it does:**
- Pins task type, target, primary metric, data + split strategy, and constraints.
- Forces an explicit definition of done — the bar the autonomous loop stops against.
- Picks the metric for the real objective and flags the class-balance/accuracy trap.

**Guardrail:** no data loading or modeling until metric and definition of done are fixed;
states assumptions or stops when the target/metric is genuinely undefined.

Built to later accept a richer input source (e.g. a Jira ticket) as the problem statement.

Trigger phrases: "frame this", "what kind of ML problem is this", "define the metric /
success criteria".
