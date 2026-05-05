---
name: Image Banks April 2026
description: Two curated image banks saved to contents/assets/ — chatbot (44 picks) + FB stories pool (74 picks). Both enriched with metadata, manifest tags, and prompt sidecars.
type: project
related: [reference_chatbot_image_bank.md, reference_model_gallery.md, reference_review_dashboard.md, project_content_distribution_system.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
Two permanent image banks curated from `contents/ready/` via model gallery (session 126). Both at `contents/assets/` with frontmatter declaring purpose + consumer.

**Why:** Image bank picker work (session 122/123 chatbot image bank reference) needed a durable store. Tying to `contents/ready/metadata.json` + `manifest.json` gives each pick a full data trail.

**How to apply:** When chatbot or story_rotation code needs to surface images, read from these files. When mutating, version-bump to `-v2` before save.

## Files

### chatbot-image-bank-2026-04.json
- **Purpose:** Chatbot image bank (messenger-chatbot channel)
- **Consumer:** `cloud-run/` chatbot Gemini handler
- **Count:** 44 picks (2 person + 2 product × 11 models)
- **Path:** `contents/assets/chatbot-image-bank-2026-04.json`
- **Rotation:** per-conversation contextual (Gemini picks by model color mentioned)

### fb-stories-pool-2026-04.json
- **Purpose:** FB Stories image pool (facebook-stories channel)
- **Consumer:** `tools/facebook/story_rotation.py`
- **Count:** 74 picks (varies per model, heavier on outback-blue)
- **Path:** `contents/assets/fb-stories-pool-2026-04.json`
- **Rotation:** time-based, every 4 hours via GH Actions cron = 6 stories/day, ~12 day cycle, ~180 stories/month

## Schema

Top-level:
```json
{
  "purpose": "...",
  "description": "...",
  "channel": "...",
  "consumer": "path/to/consumer.py",
  "rotation": "...",
  "saved_at": "YYYY-MM-DDTHH:MM:SS",
  "updated_at": "YYYY-MM-DDTHH:MM:SS",
  "count": N,
  "picks": [ ... ]
}
```

Each pick:
```json
{
  "file": "bank-bandits-blue-05-wearing.png",
  "type": "person|product",
  "model": "bandits-blue",
  "uid": "person/bandits-blue/bank-bandits-blue-05-wearing.png",
  "path": "contents/ready/person/bandits-blue/bank-bandits-blue-05-wearing.png",
  "metadata": { file, source, type, model },
  "manifest": { tags, tagged_at },
  "prompt": { ... full prompt JSON if sidecar exists ... }
}
```

## Backup lesson

Session 126 lost 16 of the original 60 FB stories picks by overwriting `chatbot-image-bank-2026-04.json` during a trim. Recovery impossible — no git history for the file, no VSCode local history (Write tool bypasses editor). **Fix forward:** bump to `-v2` before resaving a bank. See `feedback_image_bank_backup.md`.
