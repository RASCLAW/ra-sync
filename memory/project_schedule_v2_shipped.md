---
name: schedule-v2-shipped
description: "2026-05-21 session 169 -- Schedule tab v2 SHIPPED. AI Suggest chat (Sonnet 4.6, image-aware, auto-holiday context) + Calendar (PH holidays + manual events + month grid) live in CC. Supersedes schedule_tab_v2_plan."
metadata: 
  node_type: memory
  type: project
  related: 
    - project_schedule_tab_v2_plan.md
    - project_schedule_tab_v1.md
    - project_feed_scheduler.md
    - project_image_bank_overhaul.md
    - reference_schedule_chat_vs_agent_chat.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

## Status
SHIPPED. All 21 tasks from `.tmp/plan_v2.md` complete in one session. Live at `https://cc.duberymnl.com/#schedule` and `http://localhost:8090/#schedule`.

## What's live
- Top-tab pill bar `[Compose | AI Suggest | Calendar]` with localStorage-persisted last-tab restore
- **Calendar tab** — full-width month grid, prev/next/Today nav, single-style holiday chips (red) + event chips (green) + post bars (POSTED green / FAILED red / APPROVED orange), hover tooltip with position:fixed viewport clamping, click-to-select bottom panel
- **AI Suggest tab** — left column: thumb strip pulled from Compose state (reactive via `sched:images-changed` event), 5 Quick Ask presets (Draft 3 captions, Best time, Shorten, More Filipino, Check this week's PH context), Active thread meta + Reset; right column: chat card with bubbles, OPTION-card parser with Copy buttons, composer with Enter-to-send

## Files added/touched
- NEW: `references/ph_holidays_2026.json` (32 entries: official PH + commercial marketing dates)
- NEW: `references/ph_events_manual.json` (RA-managed, schema `{events:[{id,date,title,notes}]}`)
- NEW: `command-center/static/js/schedule_calendar.js`
- NEW: `command-center/static/js/schedule_chat.js`
- Modified: `command-center/app.py` (5 new routes: /calendar, /upcoming-holidays, /chat, /chat/history, /chat/reset)
- Modified: `command-center/templates/tabs/schedule.html` (3-panel structure)
- Modified: `command-center/static/js/schedule.js` (sub-tab switching + image bridge via `window.__schedState`)
- Modified: `command-center/static/css/main.css` (~150 new lines for chat + calendar + tab bar)
- Modified: `command-center/templates/shell.html` (script tags + cache bumps)

## Chat architecture
- Per-tab `session_id` (UUID in localStorage `cc.schedule.chat.session_id`)
- In-memory dict server-side: `_SCHED_CHAT_SESSIONS` = `{ messages, claude_resume_id, images_hash, token_estimate }`
- First turn embeds image paths in prompt text — Claude reads via `Read` tool (settingSources omitted, only `Read` allowlisted)
- System prompt baked: brand facts (499, no peso prefix, no model codes, 95% English) + caption/time output formats + auto-injected upcoming-holidays block fetched on each call via `_upcoming_holidays_internal(14)`
- Model: `claude-sonnet-4-6` (configurable via `SCHED_CHAT_MODEL` env)
- Cost path: Pro plan today (via claude_agent_sdk); routes to Agent SDK metered pool after 2026-06-15 cutover

## Calendar architecture
- `/api/schedule/calendar?month=YYYY-MM` merges queue posts + holidays + manual events per date
- `/api/schedule/upcoming-holidays?days=N` helper (1..60 clamped) for chat context injection
- JS module exposes `window.__schedCalendar.activate(month)` for lazy-load on tab switch
- Grid builds 6-row (42-cell) layout with previous/next-month tail fill for stable height

## Cut from original v2 plan (per RA refinement before build, captured pre-ship)
- LLM weekly events seed (`tools/calendar/refresh_events.py`) — eliminated ~$3-5/mo Anthropic cost + LLM quality risk
- Weekly Windows Task for events refresh — never registered
- Holiday tier coloring (regular vs special) — single style for all

## Bug found during build
- Regex `[-:--]+` in `parseOptionBlocks` was invalid char class (range out of order) — killed the entire `schedule_chat.js` IIFE silently. Symptom: thumbs + Ask button both dead. Fix: `[-:]+`. See [[feedback_js_regex_char_class]] for diagnostic pattern.

## Related
- [[project_schedule_tab_v2_plan]] — the plan this implemented; status now SUPERSEDED
- [[project_schedule_tab_v1]] — the Compose-only foundation v2 sits on top of
- [[project_feed_scheduler]] — the backend queue/worker this UI plugs into
- [[project_image_bank_overhaul]] — adjacent image-bank work same session
- [[reference_schedule_chat_vs_agent_chat]] — two CC chat endpoints, why they're separate
