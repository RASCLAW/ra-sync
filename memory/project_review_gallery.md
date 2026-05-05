---
name: Review Gallery System
description: DuberyMNL content library at contents/, with 5 categories + new/ staging + failed/
type: project
related: [feedback_image_review.md, feedback_ugc_tracking.md]
---

Content library at `contents/` with HTML gallery served via ngrok.

- `contents/new/` -- staging for freshly generated images (review before sorting)
- `contents/ads/` (36 WF2 ad creatives + meta JSONs)
- `contents/brand/` (20 brand content + prompts)
- `contents/product/` (9 product shots + prompts)
- `contents/ugc/` (7 UGC lifestyle images)
- `contents/carousel/` (4 carousel panels + prompts)
- `contents/failed/` -- 40 rejected images + prompts
- `contents/update_gallery.py` -- regenerates gallery data (excludes category folders)
- Gallery served via `python -m http.server` + ngrok

**Why:** Files and images were scattered across .tmp/ with no way to review or sort them.

**How to apply:** New images save to `contents/new/`. Run update_gallery.py to refresh gallery. Serve via ngrok for remote review. After review, sort into category folders or failed/.
