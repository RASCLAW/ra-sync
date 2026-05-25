---
name: feedback-veo-rai-composition
description: "Veo RAI blocks specific image compositions (wide-angle crouching + athletic wear), not text overlays or faces generally"
metadata: 
  node_type: memory
  related: 
    - feedback_veo_rai_not_faces.md
    - feedback_veo_prompting.md
    - feedback_veo_ref_image_not_supported.md
  type: feedback
  originSessionId: f29ecc51-1ba5-4419-a23a-1a504a4c3a30
---

Veo RAI filter is pose/composition sensitive, not text-sensitive. Do not assume text overlays or faces are the cause of a filter hit.

**Why:** Tested systematically in session 157:
- Brand images with heavy text overlays (e.g. "BANDITS. BOLD. BUILT." + DUBERY logo) → PASSED
- Clean studio portraits with faces (men, women) → PASSED
- `2026-05-04_bespoke_bandits_green_person_01.png` (woman, wide-angle fisheye, crouching/squatting pose, athletic wear, holding glasses up) → BLOCKED every time, support code 17301594, even with a one-line neutral prompt
- Conclusion: athletic/revealing clothing + crouching pose + wide-angle distortion = composition flag

**How to apply:** When RAI filter fires, check the image composition before assuming text or faces are the cause. Wide-angle shots with body-forward crouching/squatting poses are the known trigger. Clean studio portraits (head/shoulders, any background) are safe. If an image blocks, swap to a different angle/pose from the same shoot.
