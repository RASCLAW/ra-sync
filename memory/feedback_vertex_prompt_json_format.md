---
name: vertex-prompt-json-format
description: generate_vertex.py needs top-level "prompt" key in JSON files; passing only a nested spec (product_fidelity etc.) produces empty prompt and ERROR exit
type: feedback
related: [project_carousel_rasta_red.md, reference_vertex_ai.md, feedback_kraft_sidecar_for_fidelity.md, feedback_vertex_quota_parallel_4_blows.md, reference_cc_bespoke_pipeline.md]
---

`generate_vertex.py` reads `data.get("prompt", "")` from JSON files. A prompt file that only contains the structured spec (`product_fidelity`, `scene_variables`, etc.) at the top level produces an empty string — the generator prints `ERROR: Empty prompt text` and exits.

**Why:** Discovered during carousel generation (session 158) by reading generator source before writing prompts. Existing `_prompt.json` files in `contents/ready/` look like JSON by extension but are actually plain text — the generator's `shutil.move` renames the source prompt to `_prompt.json` regardless of original extension, so text files end up with a `.json` extension.

**How to apply:** Two valid formats for `generate_vertex.py`:

1. **JSON with `"prompt"` key** (preferred for new work):
   ```json
   {
     "prompt": "Generate an image based on...\n\n{<spec as JSON string>}",
     "image_input": ["contents/assets/product-refs/..."],
     "api_parameters": {"aspect_ratio": "4:5"}
   }
   ```
   Build these via Python `json.dumps()` to avoid manual escaping errors.

2. **Plain text `.txt` file** (legacy pattern):
   - File contains the full instruction text directly
   - Sidecar `_config.json` in same dir provides `image_input` + `aspect_ratio`
   - Generator renames to `_prompt.json` after generation

For the `.json` format, the `"prompt"` value should be: instruction header + `\n\n` + `json.dumps(spec, indent=2)`. The spec inside the string is the nested `product_fidelity`/`scene_variables` structure — it's embedded as a formatted text block within the outer JSON string.
