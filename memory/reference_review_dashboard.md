---
name: Image Review Dashboard (Stage 1 + Stage 2)
description: Two-stage image review + tagging system. Stage 1 quality gate approves/rejects, Stage 2 attaches multi-select use-case tags to a manifest
type: reference
related: [feedback_image_review.md, feedback_content_storage_rule.md, reference_cloudflare_migration.md, project_content_distribution_system.md, reference_model_gallery.md, project_image_bank_april.md]
originSessionId: 545c83ca-7691-44bd-b4f0-4e1f98f8b71e
---
Two-stage image review tool. Quality gate first, use-case tagging second. Built 2026-04-16 to clear the `contents/new/` backlog and stop story rotation from recycling the same 12 images.

## URLs (Cloudflare Tunnel)

- **Stage 1 — Quality gate:** https://review.duberymnl.com
- **Stage 2 — Use-case tagging:** https://tag.duberymnl.com

Both route through the same `dubery-chatbot` tunnel. Local ports: 8123 (Stage 1) and 8124 (Stage 2). If the tunnel is down, use local URLs directly on RA's PC.

## Files

- `tools/image_gen/image_review_recent.py` — Stage 1 server (Flask, port 8123)
- `tools/image_gen/image_tag_approved.py` — Stage 2 server (Flask, port 8124)
- `contents/ready/manifest.json` — tag manifest (written by Stage 2, read by downstream tools)

## Flow

```
generate → contents/new/    → Stage 1 review → contents/ready/  → Stage 2 tag → manifest.json → downstream consumers
                             → contents/failed/ (rejects)
```

## Stage 1 behavior

- Scans `contents/` (excluding archives/assets/failed/ready) + `.tmp/` for images modified in last N days
- Per-image Approve / Reject buttons, sticky footer submits all decisions
- Approve → `contents/ready/` (flat). Reject → `contents/failed/`. Auto-rename on name collision (`foo-v2.png`)
- Category filter chips at top: ALL / NEW / TMP / UGC / BRAND / ADS / CAROUSEL / PRODUCT
- Flags: `--days N` (default 4), `--port N` (default 8123), `--tunnel` (ngrok), `--no-tmp` (skip .tmp/)

## Stage 2 behavior

- Scans `contents/ready/` (flat files only, not subfolders)
- Reads current tags from `contents/ready/manifest.json`
- Per-image multi-select tag toggles: POST / STORY / AD / LANDING / ARCHIVE (color-coded)
- Filter chips: ALL / UNTAGGED / POST / STORY / AD / LANDING / ARCHIVE
- Orange border on cards = unsaved changes; Revert discards, Save writes manifest
- Submit writes manifest.json atomically (tmp + rename)

## Manifest schema

```json
{
  "bandits-blue-ugc-01.png": {
    "tags": ["POST", "AD"],
    "tagged_at": "2026-04-16T12:45:00"
  }
}
```

Empty tag list for an image = entry removed from manifest (not stored as `"tags": []`).

## Tag semantics

- **POST** — FB/IG feed posts
- **STORY** — story rotation pool (GH Actions cron, 12 images / 4 hours)
- **AD** — Meta Ads creative staging
- **LANDING** — duberymnl.com static assets
- **ARCHIVE** — keep for later, no immediate use (excluded from active pools)

Multi-tag allowed — an image can be POST + AD + ARCHIVE simultaneously.

## Lightbox / zoom

Click any image → full-screen viewer. Click inside = toggle 1:1 zoom. ← / → arrows = prev/next in filtered view. Esc / backdrop click / × button = close.

## Downstream wiring (planned, see project_content_distribution_system)

- Story rotation script reads manifest, picks from `STORY`-tagged images (Phase 1)
- Feed auto-poster reads `POST`-tagged, schedules via Graph API (Phase 2)
- Ad staging tool reads `AD`-tagged (Phase 2)
- Meta insights writes back to manifest (Phase 3 feedback loop)

## Portfolio framing

This is accidentally the **DuberyMNL Command Center** prototype from the backlog. Three-page dashboard (Review / Tag / Analytics) = real RAS Creative portfolio piece once analytics tab is added.
