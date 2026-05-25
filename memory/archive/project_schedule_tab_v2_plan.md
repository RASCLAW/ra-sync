---
name: schedule-tab-v2-plan
description: "2026-05-21 session 167 -- 21-task plan (~5-6 hr) for AI Suggest chat + Calendar tabs. SUPERSEDED: shipped same session as planned in session 169. See [[project_schedule_v2_shipped]] for actual delivery."
metadata: 
  node_type: memory
  type: project
  related: 
    - project_schedule_v2_shipped.md
    - project_schedule_tab_v1.md
    - project_feed_scheduler.md
    - feedback_post_ship_scope_drift.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

## Status
SUPERSEDED — shipped session 169, 2026-05-21. All 21 tasks landed. See [[project_schedule_v2_shipped]] for what's live + architecture details.

Plan file at `.tmp/plan_v2.md` and mockup at `.tmp/schedule_tab_mockup.html` retained as historical reference.

## Scope summary (locked 2026-05-21 in session 167 refinement)
- **Phase 1** (~30 min, 4 tasks): wrap existing composer in `#composerPanel`, add top-tab pill bar `[Compose | ✨ AI Suggest | 📅 Calendar]`, wire switching + localStorage persistence
- **Phase 2** (~1.5 hr, 8 tasks) -- **Calendar tab (trimmed)**: full-width month grid, single holiday chip style, post bars; hover tooltip; click → select panel. NO LLM events seed, NO weekly cron, NO Refresh-trends button.
- **Phase 3** (~3-4 hr, 9 tasks) -- **AI Suggest chat**: image-aware Claude (vision-capable Sonnet 4.6) with per-session history, preset chip composer, Reset button, option-card rendering. **Auto-injects upcoming holidays (next 14 days) into system prompt** so Claude leans into seasonal angles without RA mentioning.

## Holiday + events data sources (Calendar tab)
- `references/ph_holidays_2026.json` -- single-tier flat list, ~35 entries (official PH + commercial marketing dates: Father's Day, Mother's Day, Valentine's, 11.11, 12.12, Halloween, Black Friday). One chip style for all.
- `references/ph_events_manual.json` -- RA-managed only. Simple shape `{ "events": [{ "date", "title", "notes" }] }`. Add events as you notice them.
- Merged at `GET /api/schedule/calendar?month=YYYY-MM` endpoint with queue posts.
- New helper: `GET /api/schedule/upcoming-holidays?days=14` -- internal use by chat.

## What was cut (vs initial v2 draft)
- `tools/calendar/refresh_events.py` (LLM weekly seed script) -- gone
- `references/ph_events_seeded.json` -- gone
- Weekly Windows Task for events refresh -- never registered
- "Refresh trends" button in Calendar UI -- gone
- Holiday tier coloring (regular vs special) -- gone, one chip style
- ~$3-5/mo recurring Anthropic credit cost -- gone

## Why the cuts
RA pushed back on the LLM-seeded events layer: highest risk (LLM quality), highest cost (~$3-5/mo), highest maintenance (prompt iteration). Manual events file + holiday list covers the actual use case ("Father's Day is in 3 weeks, queue a campaign post").

## Chat session model
- Session_id per Schedule-tab-open (UUID, localStorage)
- In-memory dict keyed by session_id, stores `{ messages, image_paths_hash }`
- First turn includes image_paths as base64 content blocks; subsequent turns rely on Anthropic prompt cache (5-min TTL)
- Reset endpoint clears session

## Risks captured in plan_v2.md
- Agent SDK image upload size (3 × ~1MB)
- Prompt cache TTL (5 min) -- long pauses re-bill images
- Holiday list accuracy (proclamations shift)
- First LLM events refresh likely needs prompt iteration
- Calendar tooltip clipping on edge cells (use position:fixed)

## How to apply
- When ready to build: open `.tmp/plan_v2.md` and `/execute`
- Run Phase 2 (Calendar) before Phase 3 (Chat) -- Calendar is simpler, no LLM debugging
- Before starting, audit v1 real-use for actual gaps -- the plan may be over-scoped vs what RA actually needs

## Related
- [[project_schedule_tab_v1]] -- what's already live
- [[project_feed_scheduler]] -- backend this builds on
- [[feedback_post_ship_scope_drift]] -- why this is paused
