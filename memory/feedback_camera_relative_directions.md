---
name: Camera-Relative Directions
description: Use "left/right side of the frame" instead of clock directions -- eliminates viewer/subject POV ambiguity for Gemini
type: feedback
related: feedback_prodref_drives_direction.md, feedback_angle_aware_details.md, project_v3_fidelity_approach.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Replace all clock directions (8 o'clock, 4 o'clock, 6 o'clock) with camera-relative language:
- "Product angled toward the left side of the frame"
- "Product angled toward the right side of the frame"
- "Product facing directly toward camera"
- "Subject looking slightly downward toward the camera"

**Why:** Session 119 -- clock directions are ambiguous. "4 o'clock" from the viewer's POV is different from the subject's POV. Gemini is a visual model that thinks in frame-relative terms (left/right/center). Camera-relative language is unambiguous.

**How to apply:** Sidecar `.json` uses `frame_direction` field instead of `direction`/`compatible_directions`. Prompt's `state` and `subject_placement` must use the exact `frame_direction` string from the sidecar. Tested: left, right, down, toward camera -- all work.
