# Privacy Policy

_Last updated: 2026-06-10_

This policy covers the **claude-phoenix-skills-marketplace** plugins and skills (the
"Plugins").

## Summary

The Plugins are plain text: Markdown skill instructions, agent definitions, and JSON
manifests. They run **locally** inside Claude Code. They contain no executable telemetry, no
network calls, and no data-collection code. The maintainers of this marketplace receive
**nothing** about you or your usage.

## What the Plugins collect

**Nothing.** The Plugins do not collect, store, log, or transmit any personal data, code,
prompts, or usage information. There is no analytics, no tracking, no phone-home.

## How the Plugins operate

- Skills are instruction files loaded into your Claude Code session to guide the model's
  behavior. They execute no code of their own and open no network connections.
- The `cost-optimization` plugin defines no hooks and runs no background processes.
- The `model-router` skill may suggest delegating a subtask to a different Claude model. Any
  such model call is made by **Claude Code using your own Anthropic credentials**, directly
  to Anthropic — this marketplace is not in the path and sees none of it.

## Third parties

- **Anthropic.** When you use Claude Code with these skills, your prompts and the model's
  responses are processed by Anthropic under
  [Anthropic's Privacy Policy](https://www.anthropic.com/legal/privacy) and the terms of your
  Claude plan. This marketplace does not alter that relationship.
- **GitHub.** Installing the marketplace downloads these files from GitHub. GitHub may log
  the request per its own policy. After download, the files run locally.

## Your data

Because the Plugins collect nothing, there is nothing for us to access, retain, share, sell,
or delete. Your code and prompts never leave the path between you, Claude Code, and
Anthropic.

## Changes

Updates to this policy will be committed to this repository with a new "Last updated" date.

## Contact

Questions or concerns: open an issue at
<https://github.com/ruhaans05/claude-phoenix-skills-marketplace/issues>.
