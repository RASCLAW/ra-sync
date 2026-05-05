---
name: Chatbot Image Bank v2 (Bank-Driven)
description: contents/assets/chatbot-image-bank-2026-04.json is the source of truth for chatbot per-variant shots. knowledge_base.py loads it at import.
type: reference
related:
  - reference_chatbot_image_bank.md
  - project_chatbot_behavior_aligned.md
  - reference_chatbot_live_path.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Source of truth:** `contents/assets/chatbot-image-bank-2026-04.json` (44 picks, 4 per variant × 11 variants, ~50/50 person/product split).

**How to add/change images:**
1. Edit the bank JSON (add/remove `picks` entries — keep `file`, `type`, `model`, `path` fields)
2. Run `python tools/drive/upload_bank_to_drive.py` — uploads to Drive `DuberyMNL/Chatbot Bank 2026-04/{type}/{model}/{file}`, writes `url` + `drive_file_id` back to the bank JSON. Idempotent — safe to re-run.
3. Restart Flask — `knowledge_base.py` auto-loads the bank on import. No code change needed.

**Key patterns exposed to Gemini:**
- `{variant}` — hero shot (first product pick per variant, e.g. `bandits-green`)
- `person-{variant}-N` — on-face wearing shots (e.g. `person-bandits-green-1`, 2-3 per variant)
- `product-{variant}-N` — alt product angles beyond the default hero (e.g. `product-bandits-green-2`)

**Total image keys:** 75 (up from 42 pre-refactor). Bank-loaded: 44 (11 heroes + 23 person + 10 alt-product). Hardcoded categories unchanged: lifestyle (6), collection (4), brand (5), feedback (8), proof (6), sales (2).

**Fallback:** If a variant is missing from the bank, `knowledge_base.py` falls back to the Vercel-hosted hero at `duberymnl.com/assets/cards/*-card-shot.jpg`. Never loses coverage.

**Why this matters:** Pre-refactor, any image swap required code edits + CDN management + hardcoded URL rewrites. Post-refactor, image updates are pure content ops — edit JSON, run tool, restart.
