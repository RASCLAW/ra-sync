---
name: v3 Fidelity-Spec Approach Validated
description: Product-as-locked-asset JSON schema with interaction_physics blending layer. Validated on Outback Blue (hardest product) across ~15 scenes with consistent passes. Replaces v2 narrative prompts.
type: project
related: [project_content_skill_iterations.md, feedback_use_skills_for_content.md, project_batch001_fidelity.md, feedback_prodref_drives_direction.md, feedback_oversized_inflates.md, feedback_color_free_specs.md, feedback_angle_aware_details.md, feedback_fidelity_prefix.md, reference_kraft_prodref_workflow.md, project_outback_specs_unified.md, reference_multi_image_color_transfer.md, feedback_no_overwrite_gen.md, project_hero_prodref_categories.md, feedback_validator_ugc_scope.md, feedback_subject_placement_scene_only.md, reference_pipeline_skill_chain.md, feedback_no_headband_outfit.md]
originSessionId: aff172f2-5c57-4baa-b3b0-bc46b873440b
---
v3 fidelity-spec approach for DuberyMNL image generation. Replaces v2 narrative prompts.

**Core concept:** Product is a "locked asset" described via structural specs in JSON. Scene is the "variable." The JSON is sent directly to Gemini as structured parameters, not narrative text.

**Three-layer schema:**
1. `product_fidelity` -- identity, required_details (from product-specs.json), proportions, state
2. `interaction_physics` -- PHOTOREALISTIC_INTEGRATION blending, lighting_logic, reflection_logic (fixed), relight_instruction (fixed)
3. `scene_variables` -- location, placement, lighting, camera, objects (the creative freedom zone)

**Key rules:**
- Don't describe the emblem/logo -- Gemini reads from the reference photo
- Prodref angle drives the prompt direction -- text and image must agree
- Formatted JSON with indent=2, never one-liner
- Mandatory prefix: "Generate an image based on the following JSON parameters and the attached reference image:"
- reflection_logic is fixed: "Lenses naturally reflect the environment. Do NOT preserve reflections from the original product photo."
- relight_instruction is fixed verbatim -- prevents "pasted on" look
- No contact_points field -- Gemini handles naturally
- Don't use "oversized" in proportions -- inflates product

**Skills:** `/dubery-fidelity-prompt` (generator), `/dubery-v3-pipeline` (orchestrator), `/dubery-v3-validator` (6-check validator)
**Data files:** `product-specs.json`, `prodref-metadata.json`

**Validated on:** Outback Blue across ~15 scenes, all categories (UGC product, person-wearing, person-holding, headband, brand-model). Consistent passes on the product that failed every v2 attempt.

**How to apply:** Always use the v3 pipeline for image generation. Load product spec from file, never from memory. Run validator before spending credits.
