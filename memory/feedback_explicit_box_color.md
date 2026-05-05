---
name: Lock Box Color Explicitly in Hero-Prodref Prompts
description: For UGC_GIFTED/UNBOXING/WHAT_YOU_GET/DELIVERY, write "dark DUBERY box with red branding" in subject_placement to prevent box-color drift
type: feedback
related: [feedback_holding_product_forward.md, reference_pipeline_skill_chain.md, feedback_use_skills_for_content.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
When writing subject_placement for hero-prodref categories (UGC_GIFTED, UGC_UNBOXING, UGC_WHAT_YOU_GET, UGC_DELIVERY), include the literal phrase "dark DUBERY box with red branding" in the list of contents.

**Why:** Session 122 -- the rasta-red UGC_GIFTED shot used a "kraft paper surface" location and Gemini bled the kraft tone into the DUBERY box itself, rendering it tan instead of dark. Re-running the same category for rasta-brown WITH explicit "dark DUBERY box with red branding" language in subject_placement produced a correctly colored box even on cream-palette scenes.

**How to apply:**
- When listing package contents in `scene_variables.subject_placement`, always phrase as: "dark DUBERY box with red branding, sunglasses resting on side, microfiber pouch with red drawstring, cleaning cloth, warranty card"
- Location choices with neutral/earth-tone palettes (kraft paper, cream blanket, wooden table) are the highest risk for color bleed -- extra reason to lock explicitly
- Not needed for non-hero categories (UGC_PRODUCT, UGC_PERSON_*, UGC_FLATLAY) since the box isn't in the frame
