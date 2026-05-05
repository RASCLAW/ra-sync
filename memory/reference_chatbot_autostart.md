---
name: Chatbot Auto-Start Tasks
description: Task Scheduler at-logon tasks (DuberyMNL-Chatbot + DuberyMNL-Tunnel) auto-launch Flask + cloudflared. Install via PS1, no admin.
type: reference
related:
  - project_chatbot_recovery_complete.md
  - reference_cloudflare_worker_fallback.md
  - reference_chatbot_live_path.md
originSessionId: dcf22698-e101-4a70-8d5e-c94d2da0ed02
---
**Two Task Scheduler entries (at-logon, hidden, auto-restart on failure):**

| Task name | Runs | Batch |
|---|---|---|
| `DuberyMNL-Chatbot` | python messenger_webhook.py on :8080 | [chatbot/start-chatbot.bat](chatbot/start-chatbot.bat) |
| `DuberyMNL-Tunnel` | cloudflared tunnel run dubery-chatbot | [chatbot/start-tunnel.bat](chatbot/start-tunnel.bat) |

**Install / re-install (no admin required):**
```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File chatbot/install-autostart.ps1
```

**Verify + kick off both now:**
```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File chatbot/verify-autostart.ps1
```

**Logs:**
- `.tmp/chatbot-server.log` (Flask; renamed from `cloud-run-server.log` session 127)
- `.tmp/cloudflared.log` (tunnel)

**Uninstall:**
```powershell
Unregister-ScheduledTask -TaskName "DuberyMNL-*" -Confirm:$false
```

**Why at-logon (not service install):**
- `cloudflared service install` needs admin + UAC from Git Bash errors with "Access denied"
- `schtasks /Create` from Git Bash also errors "Access denied" due to shell elevation handling
- PowerShell's `Register-ScheduledTask` with `-RunLevel Limited -LogonType Interactive` works non-elevated and keeps the tasks user-scoped (so ADC + .env resolve correctly from RAS's profile)
- Tradeoff: tasks only fire when RAS logs in. Acceptable because laptop is always logged in when on.

**When to re-run install:**
- Task Scheduler entries disappeared (happened once — was claimed done in session 117 but tasks were missing in session 122, root cause unknown)
- After Windows reinstall / user profile reset
- After moving/renaming DuberyMNL project folder (batch files have hardcoded paths)

**Gotcha:** Git Bash mangles PowerShell `$_` pipeline variable inline — always keep PowerShell logic in `.ps1` files, not `powershell -Command "..."`.

**How to apply:** Before assuming "auto-start is wired up," run `verify-autostart.ps1` to confirm the tasks exist AND are in Ready state. If tasks are missing or disabled, run `install-autostart.ps1` to re-register.
