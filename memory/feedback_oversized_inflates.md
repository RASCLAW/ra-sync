---
name: Don't Use "Oversized" in Proportions
description: The word "oversized" in product_fidelity.proportions causes Gemini to render the product disproportionately large. Use "standard" instead.
type: feedback
related: [project_v3_fidelity_approach.md, feedback_prodref_drives_direction.md, feedback_no_scale_ref_objects.md]
originSessionId: aff172f2-5c57-4baa-b3b0-bc46b873440b
---
Never use "oversized" in the proportions field of the fidelity spec JSON.

**Why:** Session 114 -- Siargao beach hut test used "Oversized retro frame" in proportions. Product rendered disproportionately large compared to the bamboo table. Changed to "Standard retro frame" and the next test (Seoul street food) rendered at correct proportions.

**How to apply:**
- v3 validator (V3 check) scans for: oversized, large, huge, massive, giant, extra-large, enlarged
- Safe words: standard, retro, wide, compact, slim
- This applies to proportions field AND anywhere else in the prompt JSON
