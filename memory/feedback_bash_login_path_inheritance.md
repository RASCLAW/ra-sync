---
name: bash-login-path-inheritance
description: "Git Bash `-l` login shell does not reliably inherit Windows user PATH from registry. Don't rely on `claude`, `npm`, etc. being resolvable from inside `bash -l -c \"...\"`. Use full path or invoke from cmd directly."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - feedback_user_path_wipe_2026_05_20
    - project_rasclaw_telegram
  originSessionId: 9b138beb-b18d-43b3-8344-a001cda5665d
---

When a `.bat` launcher does `bash -l -c "claude --channels ..."`, the bash login shell does NOT necessarily see the Windows user PATH even when it's correctly set in the registry (`HKCU\Environment\Path`). The login profile may rebuild PATH from a stale snapshot or omit Windows entries entirely. Symptom: `/usr/bin/bash: line 1: claude: command not found` despite `setx PATH ...` having succeeded and `[Environment]::GetEnvironmentVariable('PATH','User')` returning the expected value.

**Why:** Git Bash's `-l` flag triggers `/etc/profile` + `~/.bash_profile` which may construct PATH from its own logic. The Windows user PATH update propagates to *new processes* via Explorer at next logon — but a bash login shell launched mid-session by a `.bat` from a parent process with stale env doesn't get the fresh PATH. Even after fixing user PATH, the bash layer can still drop the entries.

**How to apply:**
- For launchers that need npm-installed CLIs (`claude`, etc.) or python: invoke the binary via its **full path** from cmd, skipping bash entirely. Example from `Rasclaw/scripts/start-rasclaw.bat`:
  ```bat
  "C:\Users\RAS\AppData\Roaming\npm\node_modules\@anthropic-ai\claude-code\bin\claude.exe" --channels plugin:telegram@claude-plugins-official
  ```
- Don't trust `bash -l` to find binaries from a wrapper `.bat`. If you must use bash, pass the full path through.
- This applies broadly: any startup script that fails silently after a PATH change is suspect.
- Channel/CLI tools that need a real TTY still require launch via a VBS shim using `WScript.Shell.Run` (per [[feedback_user_path_wipe_2026_05_20]]), but the bash hop is unnecessary either way.

**First seen:** 2026-05-21 — Rasclaw TG channel restart failed twice after I fixed the PATH wipe. PATH was correct in registry but bash login shell still couldn't find `claude`. Resolved by rewriting `start-rasclaw.bat` to invoke `claude.exe` directly. Rasclaw repo commit `aec51da`.
