---
name: Trim Specs for Face-Worn Categories
description: Remove temple-arm lines from product specs when worn on face (WEARING/SELFIE/OUTFIT_MATCH) — Gemini over-renders features that go behind ears in the final scene, causing fidelity loss
type: feedback
related: [feedback_angle_aware_details.md, feedback_color_free_specs.md, project_outback_specs_unified.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
When the product is worn on someone's face in a front portrait, **temple arms are not visible** (they sit behind the ears). The sidecar `visible_details` is derived from what's visible in the *prodref photo*, NOT what's visible in the *final generated scene*. Mismatch causes Gemini to try to render features that shouldn't be in-frame, distorting the product.

**Why:** Session 126 bandits-glossy-black-01-wearing — prompt included "Slim straight glossy black temple arms, clean with no graphic patterns" even though the subject was a face-forward portrait. RA flagged it; spec was trimmed. Same issue caught on bandits-tortoise later in the session.

**How to apply:**
- Keep spec minimal: frame + lenses + bridge (3 items). Temple details belong in the prodref photo + sidecar for side-facing categories only.
- If a spec has temple-arm-specific lines, strip them for face-worn categories (WEARING/SELFIE/OUTFIT_MATCH) OR remove them from the product spec entirely if the brand is color/finish-led, not temple-detail-led.
- After removing a line from `product-specs.json`, reindex all prodref sidecars' `visible_details`: remove the deleted index, shift higher indices down. Script: iterate `[0,1,2,3]` -> `[0,1,2]` (remove 2, shift 3->2).
- Bandits-glossy-black + bandits-tortoise both had temple-arm lines removed this session. If a future batch reintroduces the same issue on a different product, repeat the trim.
