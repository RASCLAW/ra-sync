---
name: UGC Image Tracking
description: Every generated image must have its prompt JSON saved alongside it -- full traceability required
type: feedback
related: [feedback_ugc_quality.md, project_review_gallery.md]
---

All generated images must be tracked: image, prompt used, and caption linked.

**Why:** RA wants full traceability across the pipeline. Nothing should disappear into .tmp/.

**How to apply:** 
- generate_vertex.py auto-copies the prompt JSON alongside the output image (same ID, _prompt.json suffix)
- Output goes to `output/ugc/` (not .tmp/) for real runs. .tmp/ only for throwaway tests.
- Flat file naming: `UGC-20260407-001.png` + `UGC-20260407-001_prompt.json`
- Caption/metadata stays in `ugc_pipeline.json` keyed by the same ID
- Chain: caption (pipeline JSON) → prompt (_prompt.json) → image (.png)
