---
name: user-path-wipe-2026-05-20
description: "Windows user PATH (HKCU\\Environment\\Path) can get fully wiped, killing chatbot/CC/Rasclaw startup at next logon. Recovery via setx."
metadata: 
  node_type: memory
  related: 
    - reference_cc_command_center
    - feedback_python_http_server_windows
    - feedback_bash_login_path_inheritance
    - reference_cc_background_mode
  type: feedback
  originSessionId: 9b138beb-b18d-43b3-8344-a001cda5665d
---

Windows user PATH (HKCU\Environment\Path) can disappear entirely — registry key absent, not just emptied. Hit 2026-05-20 after reboot. All three startup paths silently failed:
- `DuberyMNL-Chatbot` Task Scheduler → `start-monitor.bat` → `python monitor.py` → exit code 1 (python not in PATH)
- `Startup\boot.bat` → `python command-center\app.py` → window flashes + closes
- `Startup\start-rasclaw.bat` → `bash -l -c "claude --channels ..."` → blank cmd window (bash can't find claude)

**Why:** When user PATH is wiped, Windows only loads system PATH at logon. Python lives at `C:\Users\RAS\AppData\Local\Programs\Python\Python312\` (only its `Scripts\` subdir is in system PATH). Claude CLI is the npm-installed `C:\Users\RAS\AppData\Roaming\npm\claude.cmd` (not in system PATH either). Both rely on user PATH.

**How to apply:** First diagnostic step when any startup script silently dies after reboot — run `cmd //c 'reg query HKCU\Environment /v Path'`. If "system was unable to find the specified registry value," user PATH is wiped. Recovery in one command (preferred recipe, includes Python Scripts dir for pip CLIs):

```
cmd //c 'setx PATH C:\Users\RAS\AppData\Local\Programs\Python\Python312;C:\Users\RAS\AppData\Local\Programs\Python\Python312\Scripts;C:\Users\RAS\AppData\Roaming\npm'
```

(Plain value, no surrounding quotes — setx saves literal quotes if you wrap them.) Takes effect for new processes only; current shell/Explorer keeps old env until next logon. Machine PATH already covers `C:\Program Files\nodejs` and `C:\Program Files\Git\cmd`, so no need to add them to User.

Recurrences confirmed: 2026-05-20 (initial), 2026-05-21 (Rasclaw failed at logon → claude not in PATH). Cause of recurrence unknown — may be a Windows update, an installer modifying PATH and truncating it, or registry corruption. Worth checking PATH preflight in a future Command Center health tile.

**Manual restart procedure (without waiting for logon):**
- Chatbot: `cmd //c 'schtasks /run /tn DuberyMNL-Chatbot'` — Task Scheduler spawns its own clean env.
- CC: needs detached cmd window. MINGW bash's `start ""` doesn't give claude/python a real TTY. Use a VBS shim:
  ```
  Set sh = CreateObject("WScript.Shell"): sh.Run "cmd /k bat-path", 1, False
  ```
- Rasclaw: same VBS shim — claude --channels needs a real Windows console TTY or it falls back to --print mode and errors "Input must be provided either through stdin or as a prompt argument when using --print".

**Gotcha:** `cmd //c 'start ...'` from MINGW bash will run the child cmd inline (not detached) because bash's ConPTY layer leaks through. Always use `cscript //nologo file.vbs` with WScript.Shell.Run for true Windows-side detachment.
