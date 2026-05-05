---
name: Resume Pointer (RESUME.md)
description: RESUME.md is the single source of truth for "where was I" -- overwritten by /savepoint, read first on any new session via pinned MEMORY.md link
type: reference
related: [feedback_mid_session_save.md, feedback_closeout_format.md, feedback_pro_plan_workflow.md, feedback_savepoint_sweetspot.md]
originSessionId: 8be2b90a-e08c-4a8c-bf0b-b5381c39d44c
---
`RESUME.md` lives at `C:\Users\RAS\.claude\projects\c--Users-RAS-projects-DuberyMNL\memory\RESUME.md`. `/savepoint` always overwrites it. `MEMORY.md` index has a pinned first line: `- [**RESUME HERE**](RESUME.md) -- latest savepoint, read first on new session`.

**Why:** Built 2026-04-18 session 134 as prep for Sonnet migration. Sonnet's 200K (or 1M-gated) context window is tighter than Opus[1m] -- `/compact` retains 20-40K of compressed buffer, loses nuance. `/savepoint` + `/clear` + resume from RESUME.md reloads only ~5-8K of structured state with precise cursor position. Better for Pro-plan budget mode.

**How to apply:**
- When RA runs `/savepoint`, always overwrite `RESUME.md` with: current topic, last action, **exact** next action (command or file), key file paths, blockers, in-flight tasks
- When RA types "resume" or starts a fresh session after `/clear`, MEMORY.md auto-loads -> see pinned RESUME link -> read it -> confirm state back in <10 lines -> continue from "Next action"
- RESUME.md is *not* a history log -- it's only the current cursor. Full history lives in PROJECT_LOG.md savepoint sub-entries.
- On `/closeout`, RESUME.md can be cleared or set to "SESSION CLOSED" to avoid stale pointers bleeding into next session.
