---
name: v3 PDP Description System
description: colorLabel pattern + feature image fields + 11-product spec format for v3 landing PDPs
type: project
related: [project_v3_pdp_cart_redesign.md, project_v3_pdp_gallery_editor.md, reference_v3_pdp_gallery_editor.md]
originSessionId: 009f7280-c216-46c6-9f0f-974411fd9ad7
---
colorLabel field in data.json decouples the PDP h1 title from the colorway subtitle.

**Why:** Colorway subtitle format is "Frame / Lens Polarized Lenses" (e.g. "Matte Black / Red Mirror Polarized Lenses") — first segment would make title "Outback Matte Black" instead of "Outback Red". colorLabel overrides just the color word in the title.

**How to apply:** Add `"colorLabel": "Red"` (or whatever color) to any product in data.json where the colorway first segment doesn't match the desired title color word. item.js reads `p.colorLabel || p.colorway.split(' / ')[0]`.

## Colorway format standard (session 150)
`"[Frame] / [Lens] Polarized Lenses"` — lens spec field has no "Polarized" suffix (just "Red Mirror", "Smoked", etc.)

## Feature image fields
- `feature_image`: string path → single full-width image in "The look." section
- `feature_images`: array of 2 paths → seamless dual-panel split layout (gap:0, outer-only border-radius)

Both render below the testimonials section, injected by item.js. Only products with these fields get the section.

## Products updated (session 150)
- Bandits Matte Black: colorway "Matte / Red Polarized Lenses", lens "Red Mirror"
- Bandits Glossy Black: colorway "Glossy / Smoked Polarized Lenses", lens "Smoked", feature_image BESPOKE-002
- Bandits Green: colorway "Green / Green Polarized Lenses", frame "Translucent", lens "Green Mirror", feature_image eyesonfashion
- Bandits Blue: colorway "Blue / Blue Polarized Lenses", frame "Translucent", lens "Blue Mirror"
- Bandits Tortoise: colorway "Tortoise / Smoked Brown Polarized Lenses", lens "Smoked Brown", feature_image hatbanner
- Outback Black: colorLabel "Black", colorway "Matte Black / Smoked Polarized Lenses", lens "Smoked", feature_image laughwall
- Outback Blue: colorLabel "Blue", colorway "Matte Black / Blue Mirror Polarized Lenses", lens "Blue Mirror"
- Outback Red: colorLabel "Red", colorway "Matte Black / Red Mirror Polarized Lenses", lens "Red Mirror"
- Outback Green: colorLabel "Green", colorway "Matte Black / Green Mirror Polarized Lenses", lens "Green Mirror"
- Rasta Red: colorLabel "Red", colorway "Matte Black / Red Mirror Polarized Lenses", lens "Red Mirror", feature_images dual-panel
- Rasta Brown: colorLabel "Brown", colorway "Matte Black / Smoked Brown Polarized Lenses", lens "Smoked Brown", feature_images dual-panel
