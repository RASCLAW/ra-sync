---
name: Naturalism Over Fidelity
description: Image gen prompts must prioritize natural scene integration over strict reference matching
type: feedback
related: [feedback_brand_skill_rewrite.md, feedback_plaintext_prompts.md, feedback_simple_prompts.md, feedback_ugc_quality.md, reference_vertex_ai.md, project_content_skill_iterations.md, project_v2_skills_validated.md, feedback_color_free_specs.md, feedback_fidelity_prefix.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
When prompting AI image generation with product reference images, prioritize naturalism over fidelity.

**Why:** Strict fidelity language ("MUST feature exact", "do not alter in any way") causes the model to paste/composite the reference instead of naturally recreating the product in the scene. Softer language ("matching the style shown in the reference image") produces better results because the model integrates the product into scene lighting, shadows, and physics.

**How to apply:**
- Never use "MUST feature exact" or "do not alter" -- these trigger literal copying
- Use "matching the style shown in the reference image"
- Add anti-composite language: "photographed physical object, not a digital paste or 3D render"
- Always state material finish (glossy/matte) since color descriptors are banned
- Add physics constraints: surface contact, natural positions, real shadows
- This applies to ALL image gen skills (UGC, ad creative, prompt writer)
