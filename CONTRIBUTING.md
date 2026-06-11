# Contributing

Thanks for helping grow the marketplace. Two kinds of contributions: **add a skill to an
existing collection**, or **add a whole new collection**.

## Repo model

- The repo is a **Claude Code plugin marketplace**. The root `.claude-plugin/marketplace.json`
  lists every collection.
- Each **collection** is one installable **plugin** under `plugins/<collection>/`, with its own
  `.claude-plugin/plugin.json`, an `agents/` orchestrator, and a `skills/` directory.

```
plugins/<collection>/
├── .claude-plugin/plugin.json
├── agents/<collection>-orchestrator.md   # routes the collection's skills
└── skills/<skill-name>/
    ├── SKILL.md                          # LLM-facing prompt body (+ YAML frontmatter)
    └── README.md                         # human-facing explainer
```

## Add a skill to an existing collection

1. Create `plugins/<collection>/skills/<skill-name>/`.
2. Write `SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: <skill-name>
   description: >
     What it does, and the trigger phrases that should activate it.
   ---
   ```
   Then the prompt body — concrete rules, a worked example, and a guardrails section.
3. Write a short `README.md` (what it does, why it helps, the guardrail, trigger phrases).
4. If the collection has an orchestrator agent, add a row for the new skill to its routing table.

## Add a new collection

1. Create `plugins/<collection>/.claude-plugin/plugin.json`:
   ```json
   {
     "name": "<collection>",
     "description": "...",
     "version": "0.1.0",
     "author": { "name": "ruhaans05", "url": "https://github.com/ruhaans05" }
   }
   ```
2. Add an `agents/<collection>-orchestrator.md` that diagnoses a situation and routes to the
   collection's skills (see `plugins/cost-optimization/agents/cost-optimizer.md`).
3. Add the collection's skills under `skills/`.
4. Add one entry to the root `.claude-plugin/marketplace.json` `plugins` array pointing at
   `./plugins/<collection>`.

## Quality bar

- Every skill states a **guardrail**: never sacrifice correctness for the skill's goal.
- Keep `SKILL.md` (model-facing) and `README.md` (human-facing) separate — different
  audiences.
- Don't invent model IDs, pricing, or limits. Verify against the `/claude-api` reference.
- Validate JSON before committing:
  ```bash
  python3 -c "import json,glob;[json.load(open(f)) for f in glob.glob('**/*.json',recursive=True)]"
  ```

## Test locally

```
/plugin marketplace add /absolute/path/to/this/repo
/plugin install <collection>@claude-phoenix-skills-marketplace
```

Confirm the skills and orchestrator agent appear, then open a PR.
