---
name: cc-background-mode
description: "Command Center runs hidden via pythonw.exe + boot-bg.bat + VBS shim. Logs tail to .tmp/cc.log. subprocess.Popen monkey-patched with CREATE_NO_WINDOW so child Claude CLI calls don't pop console windows."
metadata: 
  node_type: memory
  type: reference
  related: 
    - reference_cc_command_center.md
    - feedback_user_path_wipe_2026_05_20.md
    - feedback_cc_dual_instance.md
    - feedback_flask_template_cache.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

## What changed (2026-05-21 session 169)
CC previously ran in a visible `cmd /k boot.bat` window so the request log was visible. Switched to hidden mode so the cmd window doesn't sit on the desktop.

## How it launches
- `C:\tmp\launch-cc.vbs` — `WScript.Shell.Run "cmd /c boot-bg.bat", 0, False`
  - Mode `0` = hidden window
- `command-center/boot-bg.bat` — runs `pythonw.exe command-center/app.py >> .tmp/cc.log 2>&1`
  - `pythonw` is Python without a console (vs `python.exe` which always has one)
  - Stdout + stderr redirected to log file so request log + tracebacks survive

## Cmd-window flash gotcha + fix
When the parent process has no console (pythonw), Windows allocates a **new** console for every child subprocess by default. So each Claude Code CLI invocation (which the Agent SDK shells out to) was flashing a 1-second cmd window per chat call.

Fix lives in `command-center/app.py` (Windows-only startup block): monkey-patches `subprocess.Popen.__init__` to OR `CREATE_NO_WINDOW` (`0x08000000`) into `creationflags` for every child this process spawns. Works because both `subprocess.Popen` and `asyncio.create_subprocess_exec` (used by claude_agent_sdk under the hood) ultimately go through `subprocess.Popen` on Windows.

## How to restart
```
cscript //nologo C:\tmp\launch-cc.vbs
```
Or double-click the .vbs file.

To kill: `taskkill /PID <pid>` (find via `netstat -ano | grep :8090`). Or kill any `pythonw.exe` listening on port 8090.

## How to tail logs
```
tail -f C:\Users\RAS\projects\DuberyMNL\.tmp\cc.log
```
Or in PowerShell: `Get-Content -Path .tmp\cc.log -Wait -Tail 20`

## Legacy / debug launcher
`command-center/boot.bat` (uses `python.exe` with visible cmd window) is left intact for debugging — when something's broken and you want to see live tracebacks in a console, use it instead of `boot-bg.bat`.

## How to apply
- When CC seems unresponsive: check `.tmp/cc.log` first, not a missing window
- When iterating on a Python or template change: kill + restart via VBS shim (Flask debug=False caches templates per [[feedback_flask_template_cache]])
- When debugging a startup crash: use the visible `boot.bat` so you see the traceback live
- Startup folder still points at the visible `boot.bat` (per [[feedback_user_path_wipe_2026_05_20]]); swap to VBS-shim path when ready to make hidden-mode the default at logon

## Related
- [[reference_cc_command_center]] — general CC URL/port/access reference
- [[feedback_user_path_wipe_2026_05_20]] — ops recovery memory; documents the original VBS-shim pattern for `claude --channels`
- [[feedback_cc_dual_instance]] — don't run two CC processes on :8090 at once
- [[feedback_flask_template_cache]] — restart CC after any template/HTML edit (debug=False)
