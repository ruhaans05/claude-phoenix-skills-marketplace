# secret-guard

Stops secrets — API keys, tokens, passwords, private keys, `.env` values, connection
strings — from leaking into commits, logs, or output, by scanning the diff before anything
leaves the machine.

**Why it helps:** a leaked credential is hard to undo. Once it's in git history or a public
log it must be rotated. Catching it before it lands is the only cheap moment.

**What it does:**
- Scans the staged diff and content for known credential patterns and credential files.
- Keeps secrets in env vars / a secrets manager and ensures `.gitignore` rules exist first.
- Masks secrets in any output instead of echoing them.

**Guardrail:** never commits a real credential "temporarily", never weakens a secret scanner
to push faster, and treats any exposed secret as compromised — recommends rotation, not just
deletion.

Trigger phrases: "don't leak secrets", "check for keys", "is this safe to commit".
