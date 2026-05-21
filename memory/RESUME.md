---
name: session-resume-pointer
description: Latest savepoint snapshot -- read first on any new session to pick up where we left off
metadata: 
  node_type: memory
  type: project
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

# Resume Pointer

**Session:** 169 -- schedule-v2-shipped (2026-05-21)
**Status:** /savesession run — committed locally, NOT yet pushed. Run `/sendit` to ship.

## Current topic
Schedule v2 (Calendar + AI Suggest chat) + Image bank overhaul (favorites/archive/delete/thumbs/filters/drafts) + CC background mode. All landed in one session.

## What shipped
- All 21 tasks from `.tmp/plan_v2.md` — Schedule v2 fully live at `http://localhost:8090/#schedule`
- Image bank now 570 images (was 214), thumbs ~106× smaller, server-persisted favorites/archive, soft-delete to `.tmp/bank_trash/`, drafts surfaced
- CC runs hidden via `pythonw.exe` + `boot-bg.bat` + `C:\tmp\launch-cc.vbs`; logs tail to `.tmp/cc.log`
- Subprocess `CREATE_NO_WINDOW` patch in `command-center/app.py` suppresses child cmd flashes
- Content Gen got a 5th preset chip + auto-injects upcoming PH holidays into every Direction prompt

## Verification
- Schedule chat smoke: `curl -X POST /api/schedule/chat` returns "SCHED_OK" and brand-aware caption options
- Calendar API: `curl /api/schedule/calendar?month=2026-05` returns Labor Day, Mother's Day, posts on May 20
- Image bank: `/api/schedule/image-bank` returns 570 items (384 ready + 186 drafts)
- Thumb endpoint: 1.5MB PNG → 15KB JPEG confirmed
- 1 RA-scheduled post in queue: `feed-20260521-1159-001`, fires at 12:00 PM PHT cron tick

## Next action
- RA to verify the 12:00 PM PHT post lands on the FB Page
- RA to test favorites/archive/delete on real reject images
- Run `/sendit` to push + backup + sync Drive when AFK-ready
- Eventually swap Startup-folder shortcut from visible `boot.bat` to hidden VBS-shim

## Key files (this session)
- `command-center/app.py` — 5 new Schedule v2 endpoints + thumb endpoint + favorites/archive/delete + subprocess no-window patch
- `command-center/static/js/schedule_calendar.js` — NEW (calendar module)
- `command-center/static/js/schedule_chat.js` — NEW (chat module)
- `command-center/static/js/schedule.js` — sub-tab switching + image bridge
- `command-center/static/css/main.css` — calendar + chat + bank styles
- `command-center/templates/tabs/schedule.html` — 3-panel structure + lightbox + filters
- `command-center/templates/shell.html` — script tag cache bumps
- `command-center/boot-bg.bat` — NEW (hidden launcher)
- `command-center/static/js/content_gen.js` — upcoming-holidays auto-inject
- `command-center/templates/tabs/content_gen.html` — new preset chip
- `references/ph_holidays_2026.json` — NEW
- `references/ph_events_manual.json` — NEW
- `C:\tmp\launch-cc.vbs` — updated to hidden mode
- `README.md` + `PROJECT_LOG.md` — session 169 entries
- 5 new memories + backlinks updated; MEMORY.md index updated
- `decisions/log.md` — 5 new decision entries
- `~/projects/EA-brain/context/current-priorities.md` — session 169 ship note prepended
