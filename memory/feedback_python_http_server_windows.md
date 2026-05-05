---
name: Persistent Local HTTP Server on Windows
description: bash & backgrounding dies when Claude Code shell closes; use PowerShell Start-Process to keep python http.server alive
type: feedback
related: [reference_vscode_tunnel.md]
originSessionId: cedb1b48-8f09-4a9d-b7a5-8ab707d28e4c
---
Use `powershell Start-Process` to spawn a persistent local server on Windows — bash `&` backgrounding dies the moment the shell session closes.

```bash
powershell -Command "Start-Process python -ArgumentList '-m http.server 8300' -WorkingDirectory 'C:\path\to\dir' -WindowStyle Normal"
```

Verify it's up: `curl -s -o /dev/null -w "%{http_code}" http://localhost:PORT`

**Why:** Claude Code runs each Bash tool call in a temporary shell. Backgrounded processes (`&`) belong to that shell and are killed when it exits. PowerShell `Start-Process` spawns a detached Windows process that survives shell teardown.

**How to apply:** Any time RA asks to "start a local server" or "serve locally" on the Windows dev machine — always use the PowerShell form, not `python -m http.server &`.
