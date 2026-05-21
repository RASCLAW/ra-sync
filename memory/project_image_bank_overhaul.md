---
name: image-bank-overhaul
description: "2026-05-21 session 169 -- Schedule image bank rebuilt. Scans contents/ready/ + contents/new/ (570 vs prior 214). Thumb endpoint (Pillow JPEG, ~106x smaller). Favorites + Archive + Delete (server-persisted). Filters + Refresh button."
metadata: 
  node_type: memory
  type: project
  related: 
    - project_schedule_v2_shipped.md
    - project_schedule_tab_v1.md
    - reference_chatbot_bank_editor.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

## What it does now
- Scans both `contents/ready/` AND `contents/new/` (drafts get purple DRAFT badge)
- Enriches with `manifest.json` metadata (type/model/tags/tagged_at) when entry exists; untagged images still appear with `untagged: true` flag
- Newest-first by tagged_at default (falls back to file mtime for untagged)
- 240px JPEG thumbnails via `/api/thumb/<path>?w=240` (cached to `.tmp/thumb_cache/`)
- Click any tile → fullscreen lightbox with full-quality image + meta + action buttons

## Toolbar (left to right)
- Filter chips: All / Favorites / Product / Person / Brand / Drafts / Archived (dashed border)
- Refresh button — re-scans filesystem, toast shows delta count
- Model dropdown — auto-populated with all distinct `model` values + per-option counts
- Sort dropdown — Newest first / Oldest first / By model
- Search input — filename substring match
- Result count chip ("214 images") updates live as filters narrow

## Lightbox actions
- **Favorite** — toggles entry in `contents/ready/favorites.json`. Star glows on tile when favorited. Filter chip surfaces them.
- **Archive** — toggles entry in `contents/ready/archived.json`. Hides from default views; visible only via Archived chip. Reversible.
- **Delete** — confirms, then soft-moves file to `.tmp/bank_trash/<YYYY-MM-DD>/<filename>`. Cleans up favorites + archive + manifest entry. Recoverable by hand-move.
- **Cancel** / **Add to post**

## API endpoints
- `GET /api/schedule/image-bank?include_archived=0|1` — main list
- `POST /api/schedule/image-bank/archive` body `{path, action: toggle|add|remove}`
- `POST /api/schedule/image-bank/delete` body `{path}` — soft-delete to trash
- `GET /api/schedule/favorites` — list current favorites
- `POST /api/schedule/favorites` body `{path, action: toggle|add|remove}`
- `GET /api/thumb/<path>?w=240` — cached JPEG (allowed widths snap to {120,180,240,320,480,640})

## Thumbnail compression
- Pillow LANCZOS resize + JPEG quality 82, progressive
- Cache key: `sha1(path | mtime_ns | width)` so source edits auto-invalidate
- Browser-side: 30-day Cache-Control, `loading="lazy"`, `decoding="async"`
- Sample compression: 1.5MB PNG → 15KB JPEG (~106x)

## Persistence files
- `contents/ready/favorites.json` `{ "favorites": ["path", ...] }`
- `contents/ready/archived.json` `{ "archived": ["path", ...] }`
- `.tmp/thumb_cache/` (gitignored, auto-regenerated)
- `.tmp/bank_trash/<YYYY-MM-DD>/<filename>` (gitignored, soft-delete bucket)

## How to apply
- New images dropped into `contents/ready/` or `contents/new/`: click the Refresh button in the bank toolbar — server re-scans filesystem, toast shows delta count
- Restore a deleted image: `mv .tmp/bank_trash/<date>/<file> contents/ready/<wherever>/`
- Hand-edit favorites/archive lists if needed — they're just JSON arrays of project-relative paths

## Pre-warm thumb cache (deferred per RA)
Not built. Cost is zero ($0, ~15-30s CPU one-time, ~3.2MB disk) but RA opted for on-demand staggered generation. Revisit if first-open latency becomes annoying.

## Related
- [[project_schedule_v2_shipped]] — same-session adjacent work
- [[project_schedule_tab_v1]] — where the bank picker first shipped (POST-tagged-only filter, now widened)
- [[reference_chatbot_bank_editor]] — separate visual editor for the 44-pick chatbot bank
