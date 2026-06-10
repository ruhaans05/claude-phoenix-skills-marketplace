---
name: secret-guard
description: >
  Prevent secrets from leaking into commits, logs, output, or code: API keys, tokens,
  passwords, private keys, .env values, connection strings. Scan diffs before commit and
  avoid printing credentials. Use before committing/pushing, when handling config/.env, or
  when the user says "don't leak secrets", "check for keys", "is this safe to commit".
---

# Secret Guard

A leaked credential is hard to undo — once it's in git history or a public log, assume it's
compromised and must be rotated. Catching it before it lands is the only cheap moment.

## Scan before anything leaves the machine

Before `git add`/commit, before pasting output, before opening a PR, scan the diff and the
content for:

- API keys / tokens: long high-entropy strings, `sk-...`, `gho_...`, `AKIA...`, bearer tokens,
  OAuth client secrets.
- Passwords and connection strings: `password=`, `postgres://user:pass@`, `mongodb+srv://`.
- Private keys / certs: `-----BEGIN ... PRIVATE KEY-----`, `.pem`, `.p12`, `id_rsa`.
- Cloud credentials: AWS/GCP/Azure keys, service-account JSON.
- `.env`, `.env.local`, `secrets.*`, credential files — these usually should not be committed
  at all.

## The check

1. **Read the diff** (`git diff --cached`) before committing. Look for the patterns above and
   for any file that looks like a credential store.
2. **Keep secrets out of source.** Use environment variables, a secrets manager, or an
   untracked local config. Reference them; don't inline them.
3. **Ensure ignore rules exist.** `.env*`, `*.pem`, `secrets/` belong in `.gitignore` before
   the files are ever created.
4. **Don't print secrets.** Mask in logs/output (`sk-...redacted`). Never echo a full key,
   even when debugging.
5. **If a secret was already committed:** stop. Surface it. The fix is to **rotate the
   credential** — removing it from the latest commit does not undo exposure in history.

## Guardrails

- Never commit a real credential "temporarily" to test something.
- Never weaken a secret scanner (pre-commit hook, push protection) to push faster.
- A placeholder (`YOUR_API_KEY_HERE`) is fine to commit; a real value never is — check which
  one you actually have.
- Treat any secret that touched a shared channel (commit, log, chat, PR) as compromised:
  recommend rotation, don't just delete the message.
