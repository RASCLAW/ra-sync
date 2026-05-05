---
name: Outback Specs Unified Under D918 Identity
description: All 4 Outback variants (black, blue, green, red) share identical 3-field product spec -- only variant identifier differs, the prodref photo carries all variant-specific info
type: project
related: project_v3_fidelity_approach.md, feedback_color_free_specs.md, reference_kraft_prodref_workflow.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
All 4 Outback variants now share identical `required_details` in product-specs.json:
1. "Square-style matte polycarbonate frame with angular flat-top edge"
2. "Vibrant mirrored polarized lenses" (black uses "Non-mirrored polarized lenses, slightly translucent")
3. "Ensure Branding visible on the temple and spelling stays same as prod ref"

Shared `identity`: "Dubery D918 Vintage Polarized Sunglasses" (all 4 Outbacks are the D918 SKU)
Shared `proportions`: "Standard retro frame with standard hinge-to-hinge width"
Shared `finish`: "matte"
Shared `series`: "outback"

**Why:** Session 120 discovery -- all Outbacks are the same D918 model. Color differences (blue, red, green, black lenses; inner arm patterns) are carried by the kraft prodref photo, not text descriptions. Keeping the spec generic means one spec works across all variants and the photo is always the color authority.

**How to apply:** When adding new Outback variants or updating the spec, keep it generic. The kraft prodref in `contents/new/outback-{color}-kraft/{angle}.png` holds all color/pattern info.
