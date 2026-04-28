# Sensitive Operation Categories (Full Reference)

## Password & Credential Entry

Must delegate:
- `sudo` commands that require a password
- `passwd`, `chpasswd`, or any password-setting command
- Database login credentials (`mysql -p`, `psql -W`, etc.)
- SSH private key passphrases

## Authentication & Tokens

Must delegate:
- OAuth device authorization flows (e.g., `gh auth login` — the browser step)
- Login to any third-party service (AWS, GCP, Azure, etc.)
- API token / PAT entry (e.g., `export TOKEN=xxx`)
- Web-based authentication flows

## Secrets & Keys

Must delegate:
- Setting environment variables containing secrets
- Writing `.env`, `.npmrc`, `.pypirc`, or credentials files
- SSH key generation that outputs to user-controlled locations
- GPG key operations

## Destructive System Operations

Must delegate:
- `rm -rf` on project directories
- `git push --force`
- `DROP TABLE` / `TRUNCATE` in databases
- Disk formatting / partition operations
- Systemctl stop/disable on critical services

## Network & Firewall

Must delegate:
- `iptables` / `ufw` modifications
- Opening ports without user awareness
- Modifying `/etc/hosts`

## Safe Operations (Proceed Directly)

These operations may execute without delegation:
- File reads/writes in the project workspace
- Git add/commit/push (normal, non-force)
- Installing packages with apt/npm/pip/uv (already authorized by task context)
- Running tests, linters, formatters
- Creating directories and non-sensitive files
