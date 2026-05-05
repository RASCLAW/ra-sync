---
name: Model Gallery (chatbot + stories picker)
description: Flask gallery at :8125 that groups ready/ images by model with person+product sections, click-to-pick with preload from saved picks
type: reference
related: [reference_review_dashboard.md, project_image_bank_april.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
Browse-and-pick gallery for curating image banks (chatbot anchors, FB stories pool, etc). Built during session 126.

## URL
- **Local:** http://127.0.0.1:8125 (no tunnel wired)
- **Start:** `python tools/image_gen/model_gallery.py`

## File
- `tools/image_gen/model_gallery.py` — single-file Flask app (scan + serve + export)

## Behavior
- Scans `contents/ready/person/{model}/` + `contents/ready/product/{model}/` for all 11 models
- Groups by model (jump nav at top), per-model sections split into Person (blue label) + Product (green label)
- Click image to toggle select (orange border + PICK badge), double-click for lightbox with arrow nav + 1:1 zoom
- **Preload:** on page load, reads `.tmp/chatbot_image_bank_picks.json` and pre-selects those UIDs — survives gallery restarts between picking sessions
- Export button writes selected picks to `.tmp/chatbot_image_bank_picks.json` (simple array of `{uid,type,model,name}`)
- Sticky footer bar shows selected count, Clear + Export buttons

## UID format
`{type}/{model}/{filename}` — e.g. `person/bandits-blue/bank-bandits-blue-05-wearing.png`. Round-trips through export and preload.

## Downstream flow (for permanent banks)
1. Use gallery to pick images for a purpose (chatbot / stories / ads)
2. Export writes `.tmp/chatbot_image_bank_picks.json`
3. Save to `contents/assets/{purpose}-2026-{MM}.json` with frontmatter (purpose, consumer, rotation) + enriched entries (metadata + manifest + prompt)
4. If mutating, bump to `-v2` suffix first (see `feedback_image_bank_backup.md`)

## Not included
- No failed/ or new/ scan — pure ready/ browser
- No tag editing (Stage 2 tagger at :8124 handles that)
- No tunnel route — local only
