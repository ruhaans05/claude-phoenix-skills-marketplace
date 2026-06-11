# city-charter

The constitution of Agent City — the single document that defines the pipeline from one
command to a passing pull request: phase order, failure routing, iteration bounds, when
the user may be interrupted, and the laws no agent may break.

**Why it helps:** every agent in the city reads the same contract, so the pipeline behaves
the same way no matter which agent is acting. Failures route instead of ending the run;
autonomy is bounded instead of unlimited.

**What it defines:**
- The phase graph: intake → (db-consult) → blueprint → construct → (db-provision) →
  test-forge → test-run → pr-open → pr-steward → report.
- Failure routing: red tests and rejected PRs go back to `construct` with a diagnosis.
- Bounds: default 5 iterations, plateau detection, the one-pass opt-out.
- The user's peace: interrupted only for database consults and safety confirmations.

**Guardrail:** never merges to main, never claims unverified results, never widens scope,
never buries a failure in a success claim.

Trigger phrases: "build me X", "create and ship Y", "run the pipeline", "what happens next".
