---
name: UGC Image Quality Standard
description: UGC images must be premium quality with naturalism-first prompting -- recreate, don't paste
type: feedback
related: [feedback_naturalism_prompting.md, feedback_reference_angle_variety.md, feedback_ugc_tracking.md, feedback_ugc_framing.md, feedback_ra_aesthetic_preference.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
UGC content images must be premium quality -- magazine-adjacent but still authentic.

**Why:** Session 88 image review revealed that "fidelity first" prompting caused 3 failure modes:
1. Model pastes/composites the reference image (Rasta Red -- epic fail)
2. Model distorts product to show off features like logo (Outback Blue bent arm -- epic fail)
3. Model renders sunglasses as CG/3D objects (Bandits Blue -- AI-looking)

The products that PASSED (Rasta Brown 200%, Outback Red, Outback Green) had sunglasses naturally integrated into the scene.

**How to apply:**
- NATURALISM FIRST, not fidelity first -- "recreate, don't paste"
- Open prompts with: "real pair of [finish] sunglasses matching the style in the reference image -- photographed physical object, not a digital paste or 3D render"
- State material finish explicitly (glossy/matte) -- only 3 glossy models exist
- R4 Physical Realism: no floating, natural arm positions, proper surface contact/shadows
- Relax logo demands: "visible" not "must match exact placement"
- Remove "do not alter the product in any way" (triggers pasting behavior)
