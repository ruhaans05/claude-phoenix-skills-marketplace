# Contributing to Phoenix City

Thanks for helping the city grow. Three kinds of contributions: **add a skill to an
existing agent**, **add a new agent (department) to Agent City**, or **add a whole new
plugin** to the marketplace.

## Repo model

- The repo is a **Claude Code plugin marketplace**. The root
  `.claude-plugin/marketplace.json` lists every plugin.
- **Agent City** is one installable plugin under `plugins/agent-city/`: an orchestrator
  (`mayor.md`), executive agents under `agents/`, and their skills under `skills/`.

```
plugins/agent-city/
├── .claude-plugin/plugin.json
├── commands/<name>.md          # slash commands (/city, /city-status): entry points
├── agents/
│   ├── mayor.md                # orchestrator: starts/ends the pipeline, routes failures
│   └── city-<name>.md          # executive agent: owns a phase, runs its skills
└── skills/<skill-name>/
    ├── SKILL.md                # model-facing contract (+ YAML frontmatter)
    └── README.md               # human-facing explainer
```

Changes that alter behavior get a [CHANGELOG](CHANGELOG.md) entry and, when user-facing,
a version bump in `plugin.json`.

## Add a skill to an existing agent

1. Create `plugins/agent-city/skills/<skill-name>/`.
2. Write `SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: <skill-name>
   description: >
     What it does, which agent runs it, where it sits in the pipeline, and the trigger
     phrases that should activate it.
   ---
   ```
   Then the contract body: concrete rules, the output it produces, and a guardrails
   section.
3. Write a short `README.md` (what / why it helps / guardrail / triggers).
4. Add the skill to its agent's skill list, and to the Mayor's pipeline table if it's a
   pipeline phase.

## Add a new agent (department)

1. Write `agents/city-<name>.md` — frontmatter `name` + `description` (the description
   is what gets the agent invoked; make the triggers concrete), then the agent's
   jurisdiction, its skills in order, and its operating rules.
2. Add its skills under `skills/` (pattern above).
3. Wire it into the Mayor: a row in the pipeline table, and failure-routing rules in
   `city-charter` if its phase can fail.
4. Update the README's city map.

## Add a new plugin to the marketplace

1. Create `plugins/<plugin>/.claude-plugin/plugin.json` (`name`, `description`,
   `version`, `author`).
2. Build its agents + skills following the Agent City pattern.
3. Add one entry to the root `.claude-plugin/marketplace.json` `plugins` array pointing
   at `./plugins/<plugin>`.

## Quality bar — the city's laws apply to contributions too

- Every skill states a **guardrail**; every agent reports honestly (real numbers, real
  statuses, never claimed unverified).
- Nothing in the pipeline may merge to main, commit a secret, or take a destructive
  action without confirmation. New skills must not weaken these laws.
- The user is interrupted only at sanctioned points (the archivist's consult; safety
  confirmations). A new agent that needs user input must batch it the same way.
- Keep `SKILL.md` (model-facing) and `README.md` (human-facing) separate — different
  audiences.
- Validate JSON before committing:
  ```bash
  python3 -c "import json,glob;[json.load(open(f)) for f in glob.glob('**/*.json',recursive=True)]"
  ```

## Test locally

```
/plugin marketplace add /absolute/path/to/this/repo
/plugin install agent-city@phoenix-city
```

Confirm the Mayor and the executive agents appear, run a small end-to-end job ("build me
a tiny CLI that reverses a string, one pass"), then open a PR.
