---
name: YouTube API Setup
description: YouTube Data API v3 enabled in GCP, API key in .env, free quota system (10K units/day)
type: reference
related: [project_llm_wiki_system.md, reference_gcloud_cli.md, reference_youtube_skill.md]
---

YouTube Data API v3 enabled and ready.

**Config:**
- API enabled: `gcloud services list --enabled --filter="youtube"` confirms
- Key in `.env` as `YOUTUBE_API_KEY`
- Uses `google-api-python-client` (same as Sheets/Drive -- no new deps)

**Quota (free, separate from GCP credits):**
- 10,000 units/day, resets midnight Pacific
- List operations: 1 unit (video details, channel info)
- Search: 100 units (100 searches/day)
- Write/update: 50 units
- Video upload: 1,600 units (6/day)
- Does NOT consume Vertex AI / GCP credits

**Use cases:** Band research (punk aggregator), competitor analysis, trend monitoring, eventual video upload pipeline (Veo -> YouTube)

**Scope note:** Currently using API key (read-only). For uploads, would need `youtube.upload` scope added to token.json.
