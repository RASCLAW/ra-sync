---
name: Multi-Image Color Transfer for Kraft Prodrefs
description: When a product lacks a clean front supplier shot, use a sibling product's kraft front as IMAGE_0 (structure) + the target product's available supplier image as IMAGE_1 (color)
type: reference
related: reference_kraft_prodref_workflow.md, project_v3_fidelity_approach.md, feedback_color_free_specs.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

`generate_vertex.py` accepts multiple `image_input` paths. Gemini can be instructed to take pose/structure from one image and colors from another.

**Pattern (validated session 121, bandits-blue 06-front):**
```json
{
  "image_input": [
    "contents/new/bandits-green-kraft/06-front.png",
    "references/supplier-images/bandits-black-blue/01-hero.jpg"
  ]
}
```

In the prompt prefix, explicitly say:
> Use INPUT_IMAGE_0 (sibling kraft front) for the product structure, pose, framing, and frame architecture. Use INPUT_IMAGE_1 (target product supplier) ONLY for the colors. Do NOT copy the pose, angle, or composition from INPUT_IMAGE_1.

Add a `color_logic` field inside `interaction_physics`:
> "Take frame architecture and pose from INPUT_IMAGE_0. Take color palette only from INPUT_IMAGE_1: vibrant blue mirrored lenses, blue tropical pattern on arms."

**Why:** Bandits-blue supplier folder had no clean dead-on front (only 3/4 angles + back zooms). Single-image generation kept producing tilted views or fully-clear lower frames. Multi-image gave a clean dead-on front with the proper blue tint.

**How to apply:** Whenever a target product lacks a dead-on front supplier shot, find a sibling product (same frame architecture, different colors) that DOES have a clean front kraft already generated. Use that sibling's `06-front.png` as IMAGE_0, target's best 3/4 supplier as IMAGE_1. Same trick works for any "missing angle" problem -- pull pose from sibling, color from supplier.

**Architecture matches:** Bandits-green / bandits-blue / bandits-tortoise share frame design. Outback-blue / red / green / black share design. Rasta-brown / rasta-red share design. Don't cross between architectures.
