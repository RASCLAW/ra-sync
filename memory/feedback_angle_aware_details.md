---
name: Angle-Aware Required Details
description: Only include required_details that are visible in the chosen prodref angle -- mismatches cause Gemini to distort the product
type: feedback
related: [feedback_prodref_drives_direction.md, feedback_color_free_specs.md, project_v3_fidelity_approach.md, feedback_spec_trim_face_worn.md]
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
required_details in the prompt must only describe features VISIBLE in the reference angle being used. Describing features the photo doesn't show causes Gemini to hallucinate or distort the product to force those features in.

**Why:** Session 119 -- kraft prodref 06 (front view) failed when prompt included "Clean branding visible on the temple." The front angle doesn't show the temple. Gemini distorted the frame to force branding in. Removing that line fixed fidelity immediately.

**How to apply:** Each prodref has a sidecar `.json` with `visible_details` indices that map to `product-specs.json` `required_details`. At prompt time, filter required_details by these indices. For multi-image input (all angles), use full required_details since everything is visible across the set.
