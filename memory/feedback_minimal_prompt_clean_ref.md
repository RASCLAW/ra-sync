---
name: Minimal Prompt Beats Verbose When Reference Is Clean
description: When the source image is already a clean kraft prodref, a one-line instruction outperforms a full structured JSON prompt
type: feedback
related: reference_multi_image_color_transfer.md, reference_kraft_prodref_workflow.md, project_v3_fidelity_approach.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

When the source `image_input` is a clean, validated kraft prodref (already passed RA review), use a minimal natural-language prompt instead of the full structured JSON.

**Validated session 121 (rasta-brown 06-front):**
- IMAGE_0 = `contents/new/rasta-brown-kraft/01-hero.png` (clean 3/4 kraft prodref)
- Prompt = `"Make the product face towards the camera."`
- Result = perfect dead-on front, identity preserved, kraft bg consistent

**Why:** Verbose prompts compete with the reference image. When the reference already encodes the correct identity (matte frame, lens type, branding, kraft surface), Gemini interprets a short natural-language instruction as "modify only this aspect, preserve everything else." Long JSON specs can re-trigger Gemini's interpretation of every detail and cause drift.

**How to apply:** For follow-up generations off a passing kraft prodref (e.g. derive 06-front from 01-hero, derive 07-flat from 06-front), drop the full v3 schema and use one-liner instructions like:
- "Make the product face towards the camera."
- "Fold the temple arms flat behind the lenses."
- "Rotate 30 degrees clockwise around vertical axis."

For FIRST-PASS generations from a raw supplier shot (no clean kraft yet), keep the full v3 schema -- the structure prevents identity drift.
