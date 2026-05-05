---
name: Stripped Prompt Fields
description: lighting_logic and objects_in_scene are unnecessary -- Gemini handles both naturally from lighting_atmosphere and location
type: feedback
related: feedback_simple_prompts.md, project_v3_fidelity_approach.md, feedback_fidelity_prefix.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Remove `lighting_logic` and `objects_in_scene` from the v3 prompt schema. Gemini handles both naturally:
- `lighting_atmosphere` provides enough lighting context -- Gemini figures out how light hits the product
- `location` provides enough scene context -- Gemini fills in natural props/objects

**Why:** Session 119 -- tested with and without these fields. Same quality. Fewer fields = less chance of conflicting instructions. The locked phrases (relight_instruction, reflection_logic) handle the important product-light interaction.

**How to apply:** Only these fields needed in interaction_physics: blending_mode, reflection_logic, relight_instruction. Scene_variables needs: location, subject_placement, lighting_atmosphere, camera_settings. Subject block for person categories.
