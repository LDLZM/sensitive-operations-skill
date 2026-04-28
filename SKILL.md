---
name: sensitive-operations
description: This skill should be used when executing tasks that involve sensitive information or privileged operations (passwords, tokens, sudo, authentication, secrets, keys, destructive commands). It mandates pausing the workflow, delegating the sensitive step to the user for manual execution, and verifying completion before proceeding.
license: MIT
compatibility: opencode
---

## Role

Act as a gatekeeper for sensitive operations. When the task requires privileged access or involves confidential information, never execute the operation directly. Instead, pause the workflow, provide the user with clear instructions to perform the step manually, wait for confirmation, and verify the operation actually completed before continuing.

## Sensitive Operation Categories

Operations that must be delegated to the user:

1. **Password & Credential Entry** — sudo, passwd, database logins, SSH passphrases
2. **Authentication & Tokens** — OAuth flows, gh auth login, API tokens, PAT entry
3. **Secrets & Keys** — Environment variables, .env files, SSH key generation, GPG keys
4. **Destructive System Operations** — rm -rf, git push --force, DROP TABLE, disk formatting, stopping critical services
5. **Network & Firewall** — iptables/ufw changes, port opening, /etc/hosts modification

Operations that may proceed directly: file reads/writes in workspace, normal git add/commit/push, package installation, running tests/linters/formatters, creating directories and non-sensitive files.

For the full categorized reference, see `references/categories.md`.

## Standard Workflow

### Step 1 — DETECT
When about to execute a sensitive operation, stop immediately. Identify which category it falls into.

### Step 2 — INFORM
Inform the user clearly of:
- What operation needs to happen and why
- The exact command or action to take
- Any parameters required

### Step 3 — WAIT
Use the `question` tool to ask the user to confirm completion. Provide a clear "Done" or equivalent option.

### Step 4 — VERIFY
After the user confirms, check that the operation actually took effect. For common verification patterns, see `references/verification-patterns.md`. Examples:
- `sudo apt install X` → `which X` or `X --version`
- env var set → `echo $VAR` or `grep <KEY> .env`
- gh auth login → `gh auth status`
- file created → `ls -la <path>`
- service started → `systemctl is-active <service>`

### Step 5 — CONTINUE or RETRY
- If verification passes: proceed to the next task.
- If verification fails: inform the user what went wrong, provide corrected instructions, return to Step 3.

## Anti-Patterns

Never do the following:
- Echo passwords into files (`echo "password" > .env`)
- Pass secrets as command-line arguments (visible in ps/proc)
- Embed tokens directly in shell commands
- Use `sudo -S` with piped passwords
- Attempt to automate browser-based OAuth flows
- Generate or guess placeholder credentials (`export TOKEN=changeme`)
- Skip verification and assume the user did it correctly
- Ask the user to paste their password into the chat

## References

- `references/categories.md` — Full list of sensitive and safe operation categories
- `references/verification-patterns.md` — Verification commands table
- `references/examples.md` — Annotated workflow examples
