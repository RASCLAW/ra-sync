---
name: Subject Placement Scene-Only
description: subject_placement in v3 prompts must describe where the subject/product is in the LOCATION scene, never reference the prodref background (kraft paper, hero layout, etc.)
type: feedback
related: project_v3_fidelity_approach.md, reference_pipeline_skill_chain.md, project_hero_prodref_categories.md, feedback_color_free_specs.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

`scene_variables.subject_placement` must describe where the subject and/or product sit **within the LOCATION scene**. It must NEVER reference the prodref's background (e.g. "centered on the kraft paper").

**Why:** Session 121 live test -- my batch-build Python one-liner had the string "Product centered on the kraft paper" hardcoded as a fallback for kraft categories. Gemini took that literally and painted a patch of kraft paper **underneath** the product in every output (visible on outback-black UGC_PRODUCT with denim, UGC_FLATLAY with phone/watch, and as a brown paper bag artifact in UGC_PERSON_WEARING v2). Removing the "kraft paper" phrasing and re-running produced clean scenes.

**How to apply:** When writing `subject_placement`, describe:
- Person category: "Subject [wearing/holding/selfie-ing] sunglasses, {pose}, {location context} behind/around"
- Product category: "Product resting on {surface from location}, {frame_direction}"
- Hero/package category: "Dubery package on the {location surface}, all components visible as in the reference"

Never mention kraft paper, studio backdrop, or any characteristic of the prodref image's background -- those are the reference's context, not the scene's context. Gemini already uses `location` to set the scene backdrop; duplicating/contradicting it via subject_placement confuses the composition.

**Fix enforced by:** `/dubery-fidelity-prompt` skill since session 121 rewrite. Always invoke that skill for prompt building (per `/dubery-v3-pipeline` Step 4); never freelance with a Python one-liner.
