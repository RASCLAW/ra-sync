---
name: VSCode extension claude.exe orphan bug
description: VSCode Claude Code extension v2.1.100+ does not cleanly terminate claude.exe child processes when tabs are closed via X button -- orphans accumulate and eat RAM
type: reference
related: [reference_vscode_tunnel.md, feedback_loadout_remote_status.md, feedback_multi_session_safety.md, feedback_multi_session_workflow.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
The Claude Code VSCode extension (currently v2.1.100) has a known bug where closing a tab via the X button does NOT send a clean exit signal to the child `claude.exe` process. The UI panel closes but the process keeps running, holding 300-600MB RAM + any attached gdrive MCP processes.

**Why this matters:** Session 99 discovered 4 orphan claude.exe processes had accumulated over ~1 hour of tab open/close cycles during the refactor recovery. ~756MB was silently leaked. The bug was tracked as anthropics/claude-code#32792 and claimed fixed in v2.1.71, but RA's installed version is 2.1.100 (29 versions past the fix) and orphaning still occurs -- the fix was incomplete or regressed.

**Symptoms:**
- `pc-status.ps1 -SessionsOnly` shows more claude.exe processes than IDE tabs RA recognizes
- Each orphan also spawns ~200MB worth of gdrive MCP child node processes
- The gdrive MCP children typically clean up when the parent claude.exe is killed

**Important -- how NOT to exit:**
There is NO `/exit` slash command or clean exit button in the VSCode extension UI. The terminal CLI has `/exit`, but the IDE extension does not. Do not tell RA to use `/exit` in the IDE -- it doesn't exist there.

**How to kill orphans:**
1. PowerShell: `Stop-Process -Id <pid>,<pid> -Force`
2. Do NOT use `taskkill /f /pid X` from git-bash -- the `/f` flag gets mangled into a path (`F:/`) by git-bash's path translation. Use cmd.exe or PowerShell for taskkill, or use `Stop-Process` from PowerShell.
3. Nuclear option: `taskkill /f /im claude.exe` from cmd.exe, then reload VSCode window (Command Palette -> "Developer: Reload Window"). Kills everything, start fresh.

**Loadout catches this automatically:** `pc-status.ps1 -SessionsOnly` cross-references claude.exe PIDs with their JSONL file mtimes and flags anything >30 min idle as ORPHAN. Session 99 is the reference run showing this working.

**Expected until fixed upstream:** File a fresh GitHub issue if it becomes acute. For now, the orphan detection + manual kill is the stable workaround.
