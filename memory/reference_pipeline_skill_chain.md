---
name: v3 Pipeline Skill Chain (Locked)
description: Five-step chain for every v3 UGC image -- pipeline orchestrator, randomizer, fidelity-prompt builder, validator, Vertex generator. No freelancing.
type: reference
related: project_v3_fidelity_approach.md, project_hero_prodref_categories.md, feedback_subject_placement_scene_only.md, feedback_validator_ugc_scope.md, reference_kraft_prodref_workflow.md, feedback_no_headband_outfit.md, reference_multi_product_mode.md, reference_ugc_pipeline_skill.md, feedback_form_always_randomizes.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

The v3 UGC pipeline is a 5-step chain. Each step is one skill or one deterministic script. Single source of truth per step -- no freelancing at any layer.

```
/ugc-pipeline  (orchestrator skill -- /dubery-v3-pipeline archived session 122)
  ├─ Step 3: v3_randomizer.py            (scene assignment -- supports mixed-product batches)
  ├─ Step 4: /dubery-fidelity-prompt     (build prompt)
  ├─ Step 5: /dubery-v3-validator        (pre-spend gate)
  └─ Step 6: generate_vertex.py          (paid Vertex call)
```

**Responsibilities:**

| Step | Component | Owns |
|------|-----------|------|
| 3 | `tools/image_gen/v3_randomizer.py` | Category weights, location/lighting/camera/aspect banks, subject+pose construction, dedup via `layout_history.json` |
| 4 | `/dubery-fidelity-prompt` SKILL.md | v3 JSON schema, state templates (kraft + hero), CRITICAL prefix, category->prodref routing, `.tmp/*_prompt.txt` + `_config.json` output |
| 5 | `/dubery-v3-validator` SKILL.md | 8 checks: filtered spec match, naturalism, proportions, camera-relative direction, multi-image input, color-word ban, category routing, JSON schema |
| 6 | `tools/image_gen/generate_vertex.py` | Vertex API call, auto-versioning (-v2, -v3 on file exists), move prompt alongside output |

**Why this matters (session 121 bug):** When I freelanced the prompt build via a Python one-liner instead of invoking `/dubery-fidelity-prompt`, I injected "Product centered on the kraft paper" as a fallback `subject_placement`. Gemini followed literally -- kraft-paper patches appeared under products in multiple outputs. Lesson: bypass the skill chain and quality regresses. Always invoke the official skill.

**How to apply:** When running `/dubery-v3-pipeline`, do NOT batch-build prompts via scripts or inline JSON. Always invoke `/dubery-fidelity-prompt` with the randomizer assignment. Any edits to prompt shape must live inside `/dubery-fidelity-prompt` SKILL.md, not in the orchestrator or scripts.
