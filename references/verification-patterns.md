# Verification Patterns

| Operation | Verification Command |
|-----------|---------------------|
| apt install pkg | `which <cmd> && <cmd> --version` |
| gh auth login | `gh auth status` |
| env var set | `echo $<VAR>` or `grep <KEY> .env` |
| file created | `ls -la <path>` |
| service started | `systemctl is-active <service>` |
| docker installed | `docker --version && docker ps` |
| git remote set | `git remote -v` |
| npm login | `npm whoami` |
| pip/uv token set | Test with `uv pip install <pkg> --index-url ...` |
| ssh key added | `ssh-add -l` |
| systemd service | `systemctl status <service>` |
| port opened | `ss -tlnp \| grep <port>` or `lsof -i :<port>` |
| certificate installed | `openssl verify <cert>` |
| user added to group | `groups <user>` |
