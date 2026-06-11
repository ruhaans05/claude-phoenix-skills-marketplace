---
description: Report the state of the current Agent City run — ledger, branch, PR, checks
argument-hint: [optional PR number or branch name]
---

Report the state of the Agent City run in this repository ($ARGUMENTS if given,
otherwise the current branch).

1. **Read the ledger.** If `.agent-city/ledger.md` exists on the relevant branch,
   summarize it: the work order, the phase log's last entries, iterations used versus
   the cap, and decisions recorded so far. No ledger → say so, and fall back to what git
   shows.
2. **Check the ground truth.** The ledger is the run's diary, not its proof — verify
   against reality: current branch and its divergence from the default branch
   (`git status`, `git log`), whether a PR exists for it (`gh pr view`), and the live
   check status (`gh pr checks`). Where the ledger and reality disagree, report reality
   and flag the disagreement.
3. **Summarize in five lines or fewer:** phase the run is in, PR URL and check status if
   one exists, what's red and why (from real logs, not check names), iterations
   remaining, and the single next action — including "resume with /city" if the run
   stopped mid-flight.

Read-only: this command never edits files, pushes, or advances the pipeline. It only
reports.
