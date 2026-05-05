---
name: Backup System
description: Two-layer backup -- ~/.claude/ git-tracked at RASCLAW/claude-config, secrets on Google Drive.
type: reference
related: [reference_vscode_tunnel.md]
---

**Config backup:** `~/.claude/` is a git repo pushed to github.com/RASCLAW/claude-config (private).
Tracks: CLAUDE.md, settings, rules, skills, commands, agents, auto-memory.
Ignores: credentials, sessions, file-history, conversation logs.

**Secrets backup:** `python tools/drive/backup_secrets.py` uploads to Google Drive at `DuberyMNL/Backups/secrets/`.
Files: .env, credentials.json, token.json, ~/.claude/.credentials.json.
Overwrites existing on re-run.

**Recovery after SSD death:**
1. Clone claude-config from GitHub -> restore ~/.claude/
2. Clone all project repos from GitHub
3. Download secrets from Drive -> drop into project root
