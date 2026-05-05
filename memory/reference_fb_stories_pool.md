---
name: FB Stories Pool
description: contents/assets/fb-stories-pool-2026-04.json feeds tools/facebook/story_rotation.py. 74 curated picks, 4-hour rotation via GH Actions.
type: reference
related:
  - project_story_rotation.md
  - reference_chatbot_image_bank_v2.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Source of truth:** `contents/assets/fb-stories-pool-2026-04.json` (74 picks, curated for FB story rotation).

**Consumer:** `tools/facebook/story_rotation.py` — reads the bank on every run, picks index by `(hours_since_epoch // 4) % count`.

**Cadence:** GH Actions cron every 4 hours (`0 */4 * * *`). Full cycle: 74 × 4h = 296h = ~12.3 days.

**Pool breakdown (session 125):**
- 35 product / 39 person
- 5-10 picks per variant across all 11 variants
- Paths under `contents/ready/person/{variant}/` and `contents/ready/product/{variant}/`

**How to add/change:**
1. Edit `picks` in the pool JSON (add new `{file, type, model, path, ...}` entries, remove stale ones)
2. Commit + push — GH Actions reads from the repo, next cron tick picks up the change
3. No Drive upload needed (story rotation uploads to Meta per-post, doesn't need CDN URLs)

**Distinct from chatbot bank:** `chatbot-image-bank-2026-04.json` (44 picks) is for Messenger DMs. `fb-stories-pool-2026-04.json` (74 picks) is for FB story rotation. Different consumers, different cadence, different curation.

**Notes:**
- Previous version used hardcoded `STORY_IMAGES` list in `story_rotation.py` (12 images, refs were stale after reorg). Session 125 rewired to read from pool JSON.
- Feed posts (daily FB feed) are parked — doing manually for now.
