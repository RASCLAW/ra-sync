---
name: schedule-tab-v1
description: 2026-05-21 session 166 -- CC Schedule tab v1 LIVE. Compose-only mode. Single/multi/collage post + FB preview + image bank picker + 3-col queue + cron + TG pings.
metadata: 
  node_type: memory
  type: project
  related: 
    - project_feed_scheduler.md
    - reference_cc_command_center.md
    - project_schedule_tab_v2_plan.md
    - project_schedule_v2_shipped.md
    - project_image_bank_overhaul.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

## Status
LIVE at `https://cc.duberymnl.com/#schedule`. Single tab `Compose` (no top-tab structure yet -- that's v2).

## Capabilities
- **Image strip**: drag-drop reorder, × remove, click "+ Add image" opens bank picker modal
- **Bank picker modal**: thumbnail grid of POST-tagged images from `contents/ready/manifest.json`, filter chips (All/Product/Person/Brand), search by filename. Click thumb → adds to composer
- **Post modes**:
  - `multi` (default): N photos uploaded → FB picks layout (1/2/3+ photos with native swipe)
  - `collage`: pick layout template, Pillow composes one PNG, posts as single FB photo
- **Layout picker** (collage mode only): 7 templates -- 2-up, Stack, 1+2, 2x2, 3-strip, Hero+3, Before/After. Disabled state shown when image count doesn't match
- **Caption textarea** with live char count (caps at 2000)
- **datetime-local picker** for scheduled time, converts to ISO PHT (`+08:00`)
- **Live FB-style preview pane** (right column): mirrors composer state in real-time, FB grid layout for multi mode (g1/g2/g3/g4 rules), composed-image layout for collage mode
- **3-column queue grid**: Upcoming / Posted / Failed. Cards show first image thumb + `+N` stack badge if multi-image, status pill with photo count, scheduled time, relative-time label
- **Cancel button** on Upcoming cards → POST `/api/schedule/cancel`
- **Worker status indicator** top-right: "Worker last ran X min ago · posted N, failed M", dot turns warn-amber if > 80 min stale
- **Auto-refresh** every 30s when tab active

## API routes (all under `/api/schedule/`)
- `GET /queue` → `{ upcoming, posted, failed, cancelled }`, sorted appropriately, capped at 20 each
- `POST /add` → validates + appends item; returns `{ ok, id }`
- `POST /cancel` → flips APPROVED → CANCELLED; 409 if not APPROVED
- `GET /last-run` → `{ last_run_at, posted, failed }` from `.tmp/feed_worker_last_run.json`
- `GET /image-bank` → manifest entries filtered to POST tag, with src_urls

## What v1 does NOT do (deferred to v2)
- No AI Suggest chat for captions
- No Calendar view with PH holidays/events
- No image viewer + crop tool
- No top-level sub-tab structure (the `[Compose | AI Suggest | Calendar]` mockup is plan_v2.md, not built)

## Open backlog
- Hybrid Meta-native safety net (laptop-sleep insurance)
- CC external tool launcher (Canva/Photopea links)
- Plan v2 execution -- see [[project_schedule_tab_v2_plan]]

## Related
- [[project_feed_scheduler]] -- backend stack this tab plugs into
- [[project_schedule_tab_v2_plan]] -- next iteration scoped
- [[reference_cc_command_center]] -- CC URL + auth
