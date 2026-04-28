# Sensitive Operations Examples

## Example 1 — sudo password required

```
agent: sudo apt-get install -y docker.io
system: sudo: a terminal is required to read the password

agent: [STOPS — sensitive operation detected]
agent: "docker.io needs to be installed via sudo. Run this in your terminal:
  sudo apt-get install -y docker.io
Confirm when done."
user: "Done"
agent: [VERIFIES] docker --version
system: Docker version 27.0.3
agent: "Verified. Proceeding..."
```

## Example 2 — GitHub device auth

```
agent: gh auth login
system: First copy your one-time code: XXXX-XXXX
       Open this URL: https://github.com/login/device

agent: [STOPS — web auth required]
agent: "Complete GitHub authentication at https://github.com/login/device
with code XXXX-XXXX. Confirm when done."
user: "Authenticated"
agent: [VERIFIES] gh auth status
system: ✓ Logged in as username
agent: "Authentication confirmed. Proceeding..."
```

## Example 3 — Setting a secret

```
agent: [detects need to write API key to .env]
agent: [STOPS — secret handling]
agent: "Set OPENAI_API_KEY in your .env file:
  OPENAI_API_KEY=<your key>
Confirm when done."
user: "Done"
agent: [VERIFIES] grep OPENAI_API_KEY .env 2>/dev/null
system: OPENAI_API_KEY=sk-...
agent: "Key configured. Proceeding..."
```

## Example 4 — Destructive git operation

```
agent: [about to run git push --force]
agent: [STOPS — destructive operation]
agent: "Force push (git push --force origin main) will overwrite remote history.
Run this manually if you confirm, or tell me to cancel:
  git push --force origin main"
```

## Example 5 — Verification failure (retry)

```
agent: "Run: sudo systemctl start nginx"
user: "Done"
agent: [VERIFIES] systemctl is-active nginx
system: inactive
agent: "nginx did not start. Check: sudo journalctl -u nginx --no-pager | tail -20
Fix the issue and confirm when ready."
```
