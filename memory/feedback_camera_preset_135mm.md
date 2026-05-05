---
name: PERSON_WEARING Camera Preset 135mm
description: For UGC_PERSON_WEARING, use 135mm f/2.0 close portrait -- 85mm is too far (sunglasses appear oversized), 135mm macro is too close
type: feedback
related: project_v3_fidelity_approach.md, feedback_no_scale_ref_objects.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Camera preset for UGC_PERSON_WEARING: `135mm, f/2.0, close portrait shot, face fills frame, sharp focus on sunglasses and branding`.

**Why:** Session 120 iterated across three focal lengths:
- 85mm f/1.4 "tight beauty shot" -- too far; sunglasses look oversized on face, branding too small to read
- 135mm macro "extreme close-up" -- too close; loses context, looks artificial
- 135mm f/2.0 "close portrait" -- sweet spot; face fills frame, branding legible, product looks naturally sized

**How to apply:** Baked into `.claude/skills/dubery-v3-pipeline/SKILL.md` as the default PERSON_WEARING camera preset. Do NOT use "macro" or "extreme close-up" wording.
