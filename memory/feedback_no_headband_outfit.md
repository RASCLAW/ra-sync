---
name: No Headband Pose in OUTFIT_MATCH
description: UGC_OUTFIT_MATCH never uses "sunglasses on head" / perched / pushed-up poses. Worn on face or held in hand only. RA rejected headband style.
type: feedback
related: [feedback_ra_aesthetic_preference.md, project_v3_fidelity_approach.md, reference_pipeline_skill_chain.md]
originSessionId: a4a4be40-f13a-4b11-870e-502a3f27326a
---
For `UGC_OUTFIT_MATCH`, sunglasses must be either worn on face or held in hand. Never perched on head, pushed up on head, or styled as a hair accessory.

**Why:** Session 121 bandits-matte-black #1 OUTFIT_MATCH live test generated "standing full body pose, sunglasses perched on head, looking off to the side with casual confidence" — RA rejected with "i dont like the headband style." The pose bank had two headband-style options ("perched on head" + "pushed up on head") that produced a look RA considers off-brand.

**How to apply:**

- `POSES_OUTFIT` bank in `tools/image_gen/v3_randomizer.py` has both headband poses REMOVED. Remaining poses are all face-worn, collar-hanging, held-in-hand, or OOTD-style.
- OUTFIT_MATCH state template (in build scripts + pipeline SKILL.md) is now "Worn on face or held in hand, full outfit visible in frame" -- no "on head" option.
- If a future OUTFIT_MATCH pose is added to the bank, it must pass this rule: the product's role is either worn on face as eyewear, or held in hand as styled accent. Not a hair accessory.
- Fidelity-prompt + validator do not gate this explicitly — it's enforced at the randomizer bank level. If the bank gets poisoned with a headband pose again, delete it there.
