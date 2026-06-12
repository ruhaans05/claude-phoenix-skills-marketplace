---
name: groundbreak
description: >
  The city-courier's founding ceremony: make any directory — including a completely
  empty one — ready to receive the pipeline. Detects missing git, missing commits,
  missing remote, missing gh auth; founds the repo (git init, baseline commit), settles
  the GitHub question in one batched ask (create private repo / use existing remote /
  local-only), and degrades gracefully to a local-only run when GitHub isn't available.
  Use at pipeline start whenever the directory isn't a push-ready repository.
---

# Groundbreak

The pipeline's promise is "plug in anywhere and type one command" — including a folder
you created thirty seconds ago. Groundbreak is what makes that true: it inspects the
ground, founds what's missing, and never lets the run die at `pr-open` for a reason that
was knowable at minute zero.

## Inspect the ground (read-only, always first)

Run the survey before touching anything:

| Check | Command | Missing means |
|-------|---------|---------------|
| Git repo | `git rev-parse --git-dir` | found a bare folder — needs founding |
| Any commit | `git log --oneline -1` | no default branch to branch from |
| Remote | `git remote -v` | nowhere to push — needs the GitHub question |
| GitHub auth | `gh auth status` | PR phases impossible until the user logs in |

All four present → groundbreak is a no-op; log one ledger line and step aside. This
skill must cost nothing when the ground is already solid.

## Found the repo (no git / no commits)

- `git init -b main` in the project directory.
- One baseline commit so there's a default branch to branch from:
  `git commit --allow-empty -m "Found the city: baseline commit"`. Don't scaffold
  application files here — that's the engineer's jurisdiction, and the stack isn't even
  chosen until `blueprint`.
- An existing non-empty directory that simply lacks git: same moves, but the baseline
  commit stages what's already there *after* a secret sweep — never found a repo on top
  of a stray credentials file.

## Settle the GitHub question (no remote)

One batched ask — and if the archivist's `db-consult` is also pending, the Mayor merges
both into a single interruption:

```markdown
## Groundbreak
No remote is configured. Pick one (1 is the default if you don't care):
1. Create a private GitHub repo named `<folder-name>` under your account (gh repo create)
2. Use an existing remote — paste the URL
3. Local-only run — full pipeline, but it ends at a local feature branch instead of a PR
```

- Creating a repo on the user's account is outward-facing: it happens only from this
  answered consult, **private unless the user explicitly says public**.
- `gh` missing or unauthenticated → don't stall and don't try to log them in: tell the
  user the one command that fixes it (`gh auth login`), default to **local-only**, and
  continue. The run still produces built, tested, committed work.

## Local-only mode

The pipeline runs identically through `test-run`; then instead of `pr-open` →
`pr-steward`, the courier finishes the branch locally and the Mayor's report says so
plainly: branch name, commits, test numbers, and the two commands that turn it into a
PR later (`gh auth login`, `/phoenix` to resume — the ledger carries the state).

## Guardrails

- Never create a public repo, or any repo, without the answered consult. Never push
  during groundbreak — the first push is `pr-open`'s job.
- Never overwrite existing git state: a repo with history is founded ground; a weird
  HEAD or detached state gets reported, not "fixed".
- The baseline commit is empty or a swept snapshot — never invented scaffolding.
- Everything groundbreak did (or skipped) lands in the ledger and the final report:
  founded vs. found, remote created vs. local-only, and why.
