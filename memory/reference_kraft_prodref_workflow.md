---
name: Kraft Prodref Workflow
description: Generate kraft-bg product reference photos from supplier white-bg images using v3 fidelity skill -- tested and validated session 119
type: reference
related: project_v3_fidelity_approach.md, reference_product_refs.md, feedback_color_free_specs.md, project_outback_specs_unified.md, reference_multi_image_color_transfer.md, feedback_no_overwrite_gen.md, project_hero_prodref_categories.md, feedback_validator_ugc_scope.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
**Workflow:** Take supplier white-bg product photos → use v3 fidelity prompt with kraft paper scene → generate clean kraft-bg prodrefs.

**Location:** Kraft prodrefs stored in `contents/new/outback-blue-kraft/` (staging). Each `.png` gets a sidecar `.json` with angle metadata.

**Sidecar JSON format:**
```json
{
  "product": "outback-blue",
  "angle": "3/4 front-right",
  "direction": "4 o'clock",
  "compatible_directions": ["4 o'clock", "8 o'clock"],
  "shows": ["left temple arm", "branding badge", "both lenses", "frame shape", "inner arm pattern"],
  "visible_details": [0, 1, 2, 3],
  "notes": "Branding visible on left temple."
}
```

**visible_details** = indices into product-specs.json `required_details` array. Pipeline filters required_details at prompt time based on what the angle shows.

**Prompt format:** `.txt` + `_config.json` sidecar (not JSON-in-JSON). Readable and editable.

**Supplier images:** `references/supplier-images/{product}/` -- white background, scraped from duberysunglasses.com. 9 of 11 products have supplier images (no Rasta).

**Validated:** Outback Blue 6 angles generated, UGC_PRODUCT and UGC_PERSON_WEARING both pass with kraft prodrefs.
