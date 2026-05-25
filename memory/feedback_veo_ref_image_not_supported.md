---
name: feedback-veo-ref-image-not-supported
description: "--ref-image removed from generate_videos.py; RawReferenceImage is Imagen-only, Veo rejects it with 400"
metadata: 
  node_type: memory
  related: 
    - project_cc_video_tab.md
    - feedback_veo_prompting.md
  type: feedback
  originSessionId: f29ecc51-1ba5-4419-a23a-1a504a4c3a30
---

`--ref-image` / `RawReferenceImage` does not work with Veo. Remove it; do not re-add.

**Why:** `RawReferenceImage` from `google.genai.types` is an Imagen (image generation) concept. Veo's `generate_videos` API raises `400 INVALID_ARGUMENT: The image field is required for reference image` and warns `REFERENCE_TYPE_RAW is not a valid VideoGenerationReferenceType`. Tested 2026-05-17, removed from `generate_videos.py`.

**How to apply:** Veo supports only two image inputs:
- `--image` — starting frame (image-to-video; locks scene composition to that frame)
- `--last-frame` — end frame (start+end interpolation)

For scene/angle variety with product fidelity, use text-to-video and describe the product in the prompt. There is no Veo equivalent of Imagen's reference-image fidelity anchor.
