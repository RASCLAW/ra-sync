---
name: Loadout includes remote access + session health + rename prompt
description: Run remote-status.sh + pc-status.ps1 -SessionsOnly during every loadout; conditionally prompt for session rename if unnamed or multi-session
type: feedback
related: [reference_vscode_tunnel.md, reference_vscode_extension_orphan_bug.md, feedback_multi_session_safety.md, feedback_multi_session_workflow.md, feedback_session_rename_drift.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
Include remote access status **and** Claude session health in every session loadout. Also prompt for session rename when multi-window state demands it.

**Why:** RA works remotely via the dubery-dev tunnel and needs to know tunnel + power status at session start. He also accumulates orphan claude.exe processes when he closes VSCode tabs (the X button doesn't kill the child process — known bug still present in extension v2.1.100). Orphans silently eat 300-500MB each and contribute to PC lag during heavy work. Session 99 lost work when orphans piled up during a crash. On top of that: multi-window sessions rely on meaningful session names for attribution (see `feedback_multi_session_workflow.md`), and an unnamed session in a multi-window cluster defeats the point.

**How to apply:** During loadout, after git status and before asking what to work on, run BOTH scripts and include the output:

1. `bash ~/.claude/scripts/remote-status.sh` — tunnel + power status
2. `powershell -NoProfile -ExecutionPolicy Bypass -File ~/.claude/scripts/pc-status.ps1 -SessionsOnly` — session table with ACTIVE/ORPHAN labels + orphan kill command

If any sessions are flagged ORPHAN, surface them clearly and offer to kill (they're safe to terminate — JSONL files persist on disk for future `--resume`). Do not auto-kill without RA confirming which sessions he recognizes.

## Session rename prompt (conditional)

At the end of loadout, after the tunnel + session report and before asking what to work on:

- **If `pc-status -SessionsOnly` shows ≥2 active sessions** AND the current session name is default/generic (PID-based or blank) → **hard ask** for a name:
  > "Session name is `<current>`. You have N active sessions — rename this one so multi-window attribution stays clean. What should I call it? Reply with a short topic (e.g. `chatbot-recovery`, `content-pipeline`, `niche-strategy`) or `skip` if you don't know yet."

- **If only 1 active session** AND name is already meaningful → no prompt, just acknowledge in the loadout report.

- **If only 1 active session** AND name is default/generic → soft reminder:
  > "Session name is `<current>`. Consider `/rename <short-topic>` once you know what this session will be about."

## Mid-session rename (drift detection)

Initial naming at loadout is not enough — sessions drift. See `feedback_session_rename_drift.md` for proactive mid-session rename suggestions when current work clearly differs from the original named topic.
