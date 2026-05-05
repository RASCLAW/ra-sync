---
name: Prodref Angle Drives Prompt Direction
description: The reference photo's clock direction must match the prompt's product placement direction. Text and image reinforce each other -- never conflict.
type: feedback
related: [project_v3_fidelity_approach.md, feedback_oversized_inflates.md, feedback_use_skills_for_content.md, feedback_color_free_specs.md, feedback_angle_aware_details.md, feedback_camera_relative_directions.md]
originSessionId: aff172f2-5c57-4baa-b3b0-bc46b873440b
---
When generating images, the product reference photo's viewing angle DRIVES the prompt direction.

**Why:** Session 114 -- prompt said "facing 10 o'clock" but the prodref faced "6 o'clock." Gemini compromised both and the product looked wrong. When prompt direction matched the ref (e.g., both say "8 o'clock"), product fidelity held perfectly.

**How to apply:**
- Before writing a prompt, look at the prodref and determine its clock direction
- Load `prodref-metadata.json` which has direction + compatible_directions per angle file
- The prompt's `product_fidelity.state` and `scene_variables.subject_placement` must use a direction from `compatible_directions`
- Compatible directions include the mirror (8→4, 6→12, 7→5) -- Gemini can flip
- The v3 validator (V4 check) enforces this alignment
