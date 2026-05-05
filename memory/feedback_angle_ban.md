---
name: Ban -2 and -multi Angles for Image Generation
description: Multi-angle strips (-2) and composite refs (-multi) confuse Gemini into merging/distorting products. Only use single-view angles (-1, -3, -4) for generation.
type: feedback
related: [feedback_reference_angle_variety.md, reference_product_refs.md, project_content_skill_iterations.md, feedback_ra_aesthetic_preference.md]
originSessionId: 8ef4754d-4499-4e96-b43a-75efcd7fae2c
---
Multi-angle product reference images (-2.png strips and -multi.png composites) must NOT be used as generation input. Only single-view angles (-1, -3, -4) are allowed.

**Why:** Session 112 test batch (2026-04-12). UGC-012 used `bandits-green-2.png` (multi-angle strip showing 4 views). Gemini saw 4 views and didn't know which to recreate -- product fidelity failed. Regenerating with `-3.png` (single detail closeup) improved fidelity significantly. The `-2` and `-multi` refs are useful for catalog pages and chatbot image bank but cause merging/distortion when used as Gemini generation input.

**How to apply:**
- All 7 content skills updated with the ban (session 112)
- When writing prompts, only pick from `-1` (3/4 front), `-3` (detail closeup), `-4` (technical diagram)
- The `-2` and `-multi` files stay in the product-refs folder for other uses (chatbot, catalog, reference)
- Not all products have all angles -- check what exists before picking (e.g., Rasta Brown has no `-4`)
