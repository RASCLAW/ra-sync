---
name: Rasta Red Kraft Prodref Unreliable
description: The 01-hero kraft prodref for rasta-red renders gold/amber mirrored lenses in collection compositions instead of the expected red mirror. Causes fidelity drift when Rasta Red is included in mixed-product batches.
type: project
related: [project_brand_collection_formula.md, reference_kraft_prodref_workflow.md, project_batch001_fidelity.md, feedback_prompt_bloat_fidelity.md]
originSessionId: 4c8d3a18-7f47-44f5-a26a-1a841fdc1451
---
The `contents/assets/prodref-kraft/rasta-red/01-hero.png` reference image produces inconsistent lens color in Gemini outputs when used in multi-product brand collection compositions. Expected per product-specs: "Vibrant mirrored polarized lenses" (red-toned). Actual in renders: gold/amber/yellow mirror tones, especially in warm-lit or flat-studio mixed scenes.

**Why:** Session 128 brand-coll batch B3 task 009 (5-up cross row on anodized aluminum, flat studio lighting) rendered Rasta Red with yellow/gold lenses. Task 011 (UNBOX exploded flat-lay on brushed gunmetal) showed similar drift. HC3 (Rasta Brown hero + Bandits Tortoise + Outback Red) rendered the Rasta Brown hero as a slim-square (Bandits) shape rather than the rounded (Rasta) profile the prodref shows. Pattern: when Rasta products are mixed with Bandits in the same frame, Gemini appears to bias toward the more-common slim-square silhouette, and lens color drifts warm.

RA hypothesis: the prodref itself may be underspecified — possibly the kraft photo's ambient was warm enough that Gemini reads the lens as warm-mirrored rather than red-mirrored. Sidecar `01-hero.json` notes: "Generated via multi-image color transfer (rasta-red kraft structure + rasta-brown product-ref color)" — suggests the reference was synthetically assembled and may lack strong red cues.

**How to apply:** Until the rasta-red prodref is regenerated:
1. Be cautious when including rasta-red in mixed-product batches (expect fidelity drift on lens color and sometimes frame shape).
2. Options to mitigate: (a) regenerate the kraft prodref with a cleaner red mirror source image; (b) isolate Rasta Red to Rasta-only scenes where warm-light bias doesn't compound with mixed-shape confusion; (c) as last resort, add minimal lens-color cue to the fidelity block (risks R2 color-word violation per `feedback_color_free_specs`).
3. Backlog: "Regenerate rasta-red kraft prodref for clearer red-mirror rendering" — add to current-priorities next time the prodref bank is revisited.

Same reliability audit recommended for rasta-brown prodref, which also uses multi-image color transfer and may share the issue.
