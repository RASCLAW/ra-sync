---
name: Multi-session safety -- don't commit files from another active session
description: When another Claude Code session is actively editing files in the same project, do NOT commit those files -- leave them unstaged for the other session
type: feedback
related: [feedback_loadout_remote_status.md, reference_vscode_extension_orphan_bug.md, feedback_multi_session_workflow.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
**Rule:** When RA has another active Claude Code session editing files in the same project, do NOT commit those files. Leave them unstaged for the other session to handle in its own scope.

**Why:** Session 99 almost committed 3 cloud-run/ files (conversation_engine.py, knowledge_base.py, security.py) plus 2 deleted files -- all active work-in-progress in RA's other IDE tab. Three risks of committing another session's work:
1. Committing incomplete or broken work-in-progress
2. Creating merge conflicts when the other session commits its own version
3. Overwriting the other session's uncommitted work if it's actively writing

The loadout orphan detection (`pc-status.ps1 -SessionsOnly`) surfaces all active sessions with idle time -- this makes multi-session situations detectable at session start, not mid-commit.

**How to apply:** Before staging a commit:
1. Check active sessions via `pc-status.ps1 -SessionsOnly`
2. If more than one ACTIVE session exists, ASK RA which files belong to which session before staging
3. When uncertain, unstage and ask per-file
4. Watch for "file just opened in IDE" context hints -- that's usually the other session's focus area (e.g. session 99 RA opened cloud-run/security.py in the IDE, confirming that was the other session's work)
5. `settings.local.json` is shared state between all sessions -- always unstage when multiple sessions are active
