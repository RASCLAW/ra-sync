---
name: kraft-sidecar-for-fidelity
description: When generating ad creatives via Vertex/generate_vertex.py, use kraft prodref + sidecar JSON (frame_direction + visible_details) instead of raw multi-angle product-refs. Visibly improved fidelity.
type: feedback
related:
  - feedback_fidelity_anchor.md
  - feedback_vertex_prompt_json_format.md
  - feedback_bespoke_concept_paste_wins.md
---

For Vertex image generation that locks product identity, pass `contents/assets/prodref-kraft/{sku}/01-hero.png` as image_input[0] AND filter product-specs `required_details` by the sibling sidecar's `visible_details` array. Use the sidecar's `frame_direction` string in the product "state" field. Append the fidelity anchor line as the final `required_detail` ([[feedback_fidelity_anchor]]).

**Why:** Discovered in session 170 batch comparison. Batch 1 (raw `product-refs/{sku}/{sku}-N.png` photos) had visible fidelity drift -- wrong lens tints, frame geometry distortion, asking for views the reference angle doesn't show. Batch 3 (kraft + sidecar) produced visibly better fidelity on the same SKUs and prompts: Bandits Blue's tropical temple pattern showed up correctly, Rasta Brown's accent stripe stayed locked, Outback frames stayed square-edged. Kraft is the canonical studio reference; sidecar tells the agent which details are physically visible from the reference angle so it stops inventing things ("underside of upper rim" in batch 3 v3 forced Gemini to invent an angle the prodref doesn't show).

**How to apply:**
- Always load `contents/assets/prodref-kraft/{sku}/01-hero.json` (or 06-front.json for front-on PRODUCT anchors) alongside the kraft image
- Filter `SPECS[sku]["required_details"]` by sidecar `visible_details` indices before passing to the prompt
- Append the verbatim fidelity anchor as the final item (see [[feedback_fidelity_anchor]])
- Reference the sidecar's `frame_direction` in the prompt's product "state" field with phrasing like "Product angled slightly toward the right side of the frame, matching the reference photo orientation"
- For bundles, repeat for each SKU (image_input[0] = sku_a kraft, image_input[1] = sku_b kraft)
- `tools/image_gen/generate_vertex.py` already accepts this shape -- just structure the prompt JSON accordingly (see [[feedback_vertex_prompt_json_format]] for the JSON shape it expects)
