# groundbreak

The city-courier's founding ceremony: makes any directory — including a completely empty
one — ready to receive the pipeline. Inspects for git, commits, remote, and `gh` auth;
founds what's missing; settles the GitHub question in one batched ask.

**Why it helps:** "plug it into an empty IDE and ask it to build" only works if the
pipeline doesn't die at `pr-open` because nobody ran `git init`. Groundbreak moves that
discovery to minute zero and handles it — or degrades gracefully to a local-only run
that still produces built, tested, committed work.

**What it does:**
- Empty folder → `git init -b main` + empty baseline commit. Existing files → same, with
  a secret sweep before the first commit.
- No remote → one ask, merged with the database consult if both are pending: create a
  private GitHub repo (default), paste an existing remote, or go local-only.
- No `gh` auth → names the fix (`gh auth login`), defaults to local-only, keeps going.
  The ledger carries the state so `/phoenix` later upgrades the run to a real PR.
- Push-ready repo already → no-op, one ledger line.

**Guardrail:** never creates a repo without the answered consult (private unless
explicitly public), never pushes (that's `pr-open`'s job), never overwrites existing git
history, never invents scaffolding — the baseline commit is empty or a swept snapshot.

Trigger phrases: runs automatically at pipeline start when the directory isn't a
push-ready repository.
