# Privacy Policy

_Last updated: 2026-06-11_

This policy covers the **Phoenix City** marketplace and the **Agent City** plugin — its
agents, skills, and manifests (the "Plugins").

## Summary

The Plugins are plain text: Markdown agent definitions, Markdown skill instructions, and
JSON manifests. They run **locally** inside Claude Code. They contain no executable
telemetry, no network calls of their own, and no data-collection code. The maintainers of
this marketplace receive **nothing** about you or your usage.

## What the Plugins collect

**Nothing.** The Plugins do not collect, store, log, or transmit any personal data, code,
prompts, credentials, or usage information. There is no analytics, no tracking, no
phone-home.

## How the Plugins operate

- Agents and skills are instruction files loaded into your Claude Code session to guide
  the model's behavior. They execute no code of their own and open no network connections.
- The `agent-city` plugin defines no hooks and runs no background processes.
- Actions the pipeline performs — writing files, running tests, creating branches,
  opening pull requests, connecting to a database — are performed by **Claude Code on
  your machine, with your own tools and your own credentials** (your `git`/`gh`
  authentication, your database credentials, your Anthropic account). This marketplace is
  not in the path and sees none of it.

## Credentials and your database

The `city-archivist` agent may ask you for database credentials, connection strings, or
API keys during its consult. These are given by you directly to your local Claude Code
session. The Plugins' own instructions require that credentials be kept in local
environment files (gitignored), never committed, and never echoed back — but the
credentials themselves live only on your machine and in your session with Anthropic. The
maintainers of this marketplace never receive them.

## Third parties

- **Anthropic.** When you use Claude Code with these Plugins, your prompts and the
  model's responses are processed by Anthropic under
  [Anthropic's Privacy Policy](https://www.anthropic.com/legal/privacy) and the terms of
  your Claude plan. This marketplace does not alter that relationship.
- **GitHub.** Installing the marketplace downloads these files from GitHub. The pipeline
  also pushes branches and opens pull requests **to repositories you choose, using your
  own GitHub authentication** — that traffic is between you and GitHub under
  [GitHub's Privacy Statement](https://docs.github.com/site-policy/privacy-policies/github-privacy-statement).
- **Database / hosting providers.** If you direct the archivist at a hosted database
  (e.g., Supabase, Neon, MongoDB Atlas), your app's connection to it is governed by that
  provider's policy and your account with them. This marketplace is not involved.

## Your data

Because the Plugins collect nothing, there is nothing for us to access, retain, share,
sell, or delete. Your code, prompts, and credentials never pass through this marketplace.

## Changes

Updates to this policy will be committed to this repository with a new "Last updated"
date.

## Contact

Questions or concerns: open an issue at
<https://github.com/ruhaans05/claude-phoenix-skills-marketplace/issues>.
