---
name: Carousel Format Rules
description: Connected carousel images -- generate wide 2:1 or 3:1 and slice, product-anchor only, not person-anchor
type: feedback
related: [feedback_brand_skill_rewrite.md, project_brand_pipeline.md]
---

Connected carousel images work by generating one wide image and slicing into equal panels.

**Why:** RA tested wide format carousels. Product close-ups create "swipe to see more" curiosity. Person-anchor in wide format doesn't create that pull -- you already see everything.

**How to apply:**
- 2-panel carousel: generate at 2:1 ratio, slice in half into 1:1 panels
- 3-panel carousel: generate at 3:1 ratio, slice into thirds
- Carousel = product-anchor ONLY (collections, features, packaging)
- Person-anchor content stays single frame (4:5 feed, 9:16 stories)
- Tell prompt to keep key text/products away from cut lines
- Slicer is simple Pillow crop -- already tested and working
