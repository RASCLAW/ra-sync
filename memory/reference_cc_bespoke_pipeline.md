---
name: cc-bespoke-pipeline-layout
description: CC Content Gen bespoke flow -- file layout, prompt JSON structure, where outputs go. Reference for any tooling that integrates with the bespoke pipeline.
type: reference
related:
  - feedback_bespoke_concept_paste_wins.md
  - feedback_kraft_sidecar_for_fidelity.md
  - feedback_vertex_prompt_json_format.md
  - reference_cc_command_center.md
---

When RA uses the CC Content Gen tab in bespoke mode (Direction box + pasted concept image + settings: bespoke/product/ratio/sku), the pipeline writes outputs to a dated run folder.

## Output structure

Each run lands in: `contents/runs/{YYYY-MM-DD_HHMMSS}_bespoke/`

Contains:
- `{date}_{sku}_bespoke_{conceptname}.png` -- the generated image
- `{date}_{sku}_bespoke_{conceptname}_prompt.json` -- the structured prompt JSON fed to `generate_vertex.py`
- `run.json` -- session metadata (direction text, settings, image paths, prompt paths)

## Prompt JSON structure (the bespoke skeleton)

The structured prompt JSON has these top-level keys (same skeleton as the v3 fidelity-prompt skill):

```
product_fidelity:
  identity              # from product-specs.json
  required_details      # filtered by kraft sidecar visible_details + fidelity anchor appended
  proportions           # from product-specs.json
  state                 # CONCRETE design move extracted from reference, e.g. "Oversized hero product anchored top-right, tilted on a strong diagonal..."

interaction_physics:
  blending_mode         # always PHOTOREALISTIC_INTEGRATION
  reflection_logic      # tells Gemini lens reflects the new scene, not the original prodref
  relight_instruction   # "Use INPUT_IMAGE_0 but digitally relight to match the new location"

scene_variables:
  location              # CONCRETE design context, e.g. "Clean studio with gradient backdrop sky-blue top-left to peach-pink bottom-right"
  subject_placement     # CONCRETE composition + typography placement (the key field for ad-creative quality)
  lighting_atmosphere
  camera_settings

render_quality:
  resolution: high
  color_space: true-to-life
  background_blur_strength

image_input              # array of paths -- kraft prodref + (optionally) font alphabet + logo
api_parameters:
  aspect_ratio           # 1:1, 4:5, 9:16 etc.
```

The whole thing is wrapped under a top-level `"prompt"` string key when fed to `generate_vertex.py` (see [[feedback_vertex_prompt_json_format]]).

## run.json structure

```
{
  "ts": "2026-05-22T07:52:29",
  "mode": "bespoke",
  "type": "product",          # or "person"
  "count": 1,
  "products": ["bandits-matte-black"],
  "direction": "<RA's prompt text + agent's interpretation>",
  "aspect_ratio": "1:1",
  "images": [<paths>],
  "prompts": [<paths>],
  "concepts": [<reference image paths if pasted>]
}
```

## Why this matters

The bespoke flow's quality jump vs. randomized text-to-image batches comes from `subject_placement` + `location` containing CONCRETE design moves (extracted from the reference image by the agent), not abstract scene strings like "Manila BGC rooftop bar with afternoon city skyline." See [[feedback_bespoke_concept_paste_wins]].

Existing portfolio: `contents/ready/ads/` has ~120 outputs from this flow (sessions across April-May 2026). Use as the quality reference standard.

## How to integrate

Any tool that wraps the bespoke pipeline (e.g., the planned [[project_inspiration_batch_ingestion]] batch_bespoke.py) should:
- Write outputs to `contents/runs/{timestamp}_bespoke/` (or `_batch_bespoke/` for batch runs) to match CC convention
- Emit the same prompt JSON shape so it shows up in CC's history view
- Use kraft + sidecar for image_input[0] ([[feedback_kraft_sidecar_for_fidelity]])
