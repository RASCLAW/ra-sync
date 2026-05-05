---
name: GWS CLI Setup
description: Google Workspace CLI installed globally, auth at ~/.config/gws/, known POST bug #188
type: reference
related: [reference_gmail_labels.md, reference_token_scopes.md]
---

GWS CLI (v0.22.5) installed via `npm install -g @googleworkspace/cli`.

- Auth config: `~/.config/gws/` (client_secret.json copied from DuberyMNL credentials.json)
- Auth command: `gws auth login --services gmail` (run in user terminal, not Claude Code)
- Scopes: gmail.modify, gmail.settings.basic + standard
- Known bug: POST requests with `--json` return 403 despite valid scopes (Issue #188). Use Python Gmail API as workaround for write operations.
- GET commands work fine (labels list, triage, etc.)
- Gmail API enabled in GCP project duberymnl-automation
