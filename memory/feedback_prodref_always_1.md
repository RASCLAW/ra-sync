---
name: Always use -1.png prodref
description: Hardcode image_input to the -1.png angle variant for all products in v3 pipeline
type: feedback
related: [feedback_prodref_drives_direction.md, project_v3_fidelity_approach.md, feedback_reference_angle_variety.md]
originSessionId: f211a511-dd7f-4cd8-bc62-1b65e2fa1101
---
Always pick the `-1.png` product reference photo for image generation. Do not randomize across -2, -3, etc.

**Why:** RA was getting repetitive-looking results when using -2.png (front-facing). The -1 angle (8 o'clock, 3/4 view) shows the temple arm and branding, giving more visual interest and product recognition.

**How to apply:** In the v3 pipeline randomizer (Step 1b), skip angle randomization -- always select `{product}-1.png`. Set direction from that file's `compatible_directions` (typically 8 o'clock or 4 o'clock).
