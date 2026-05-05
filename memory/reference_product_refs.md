---
name: Product Reference Photos
description: All 11 product variants with multi-angle refs at contents/assets/product-refs/, plus logos and fonts
type: reference
related: [feedback_reference_angle_variety.md, project_product_catalog.md, project_slate_cards.md, reference_product_finish.md, reference_kraft_prodref_workflow.md]
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
## Location
`contents/assets/product-refs/` (inside DuberyMNL repo)

## Available -- All 11 Variants
- bandits-blue/ (4 angles)
- bandits-glossy-black/ (3 angles)
- bandits-green/ (5 -- 4 angles + multi)
- bandits-matte-black/ (5 -- 4 angles + multi)
- bandits-tortoise/ (4 angles)
- outback-black/ (4 -- 3 angles + multi)
- outback-blue/ (4 -- 3 angles + multi)
- outback-green/ (4 -- 3 angles + multi)
- outback-red/ (4 -- 3 angles + multi)
- rasta-brown/ (4 -- 3 angles + multi)
- rasta-red/ (4 -- 3 angles + multi)

## Naming Convention
`{model}-{number}.png` for individual angles, `{model}-multi.png` for multi-angle strips.

## Other Assets
- Logos: `contents/assets/logos/`
- Fonts: `contents/assets/fonts/`

## Usage
Pass as `Part(inline_data=Blob(...))` before the text prompt. Use single angle per product to avoid ghost pairs.
