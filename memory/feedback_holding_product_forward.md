---
name: HOLDING Shots Are Product-Forward
description: UGC_PERSON_HOLDING must frame sunglasses close to camera lens, product dominating the frame, subject secondary
type: feedback
related: [feedback_ugc_framing.md, reference_pipeline_skill_chain.md, project_content_skill_iterations.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
All UGC_PERSON_HOLDING prompts must show the sunglasses held close to the camera lens with the product dominating the composition. Subject face is softly blurred or partially visible behind the product.

**Why:** Session 122 -- a rasta-red HOLDING shot rendered as a chest-up portrait with the sunglasses small in hand. RA flagged: "i want the holding shot to focus on the sunglasses more." The whole point of HOLDING vs WEARING is to hero the product in hand, not to be another face portrait.

**How to apply:**
- POSES_HOLDING in `tools/image_gen/v3_randomizer.py` rewritten to force close-to-camera poses ("extended close to the camera lens", "filling most of the frame", "pushed forward toward the lens")
- CAMERAS["UGC_PERSON_HOLDING"] presets all emphasize product-forward framing ("sunglasses filling most of the frame", "sunglasses as the clear focal point")
- When writing subject_placement in the prompt, match language: product dominates, face secondary/blurred behind
