---
name: Meta Saved Audience Targeting API Fix
description: saved_audiences ID is not valid in Meta API targeting spec -- must fetch and store full targeting dict
type: feedback
related: reference_meta_attachment_caching.md, project_meta_catalog.md
---

Meta's `saved_audiences` field is NOT accepted in the ad set targeting spec via Graph API, even though the saved audience ID exists in Ads Manager.

**Why:** The API requires explicit targeting params (geo_locations, age, interests, behaviors). The saved audience ID only works in the UI.

**How to apply:** For any saved audience preset, fetch the full targeting spec via:
```
GET /{audience_id}?fields=id,name,targeting&access_token=...
```
Store the result under `command-center/presets/marketing.json` → audience → `targeting` key.
`build_targeting_from_preset` in `stage_creatives.py` now reads this directly.

If a new saved audience is added to presets without a `targeting` field, the script will raise a clear error asking to fetch it.
