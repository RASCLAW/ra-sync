---
name: Color-Free Product Specs
description: Remove all color words from required_details -- Gemini reads color from the reference photo, text colors can conflict
type: feedback
related: feedback_naturalism_prompting.md, project_v3_fidelity_approach.md, feedback_prodref_drives_direction.md, project_outback_specs_unified.md, reference_multi_image_color_transfer.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Never include color words (blue, black, green, red, gold, amber, grey, brown) in `required_details` of product-specs.json. Describe structure and type only: "mirrored polarized lenses" not "blue mirrored polarized lenses", "matte polycarbonate frame" not "matte black polycarbonate frame".

**Why:** Session 119 test proved Gemini reads lens/frame color from the reference photo. Removing "blue" from required_details still produced blue lenses. Color words in text can conflict with the reference image and cause Gemini to compromise both inputs.

**How to apply:** When writing product specs or prompt required_details, describe: material (matte/glossy/polycarbonate), type (mirrored/polarized/non-mirrored), shape (square-style/rounded/angular flat-top). Never describe color -- the prodref photo is the color authority.
