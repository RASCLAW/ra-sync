---
name: No Scale-Reference Objects in Product Shots
description: Avoid placing product next to objects with recognizable real-world size (newspapers, phones, books) -- product renders oversized
type: feedback
related: feedback_oversized_inflates.md, feedback_narrow_scenarios.md, feedback_no_night_scenes.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
For UGC_PRODUCT surface shots, avoid placing the product next to objects that have a known real-world size. Gemini renders the product to fill the frame regardless of what's beside it, making it look oversized.

**Fails:** Newspapers (30cm vs 14cm sunglasses), vinyl records (30cm), phones, books, wallets
**Works:** Clean surfaces (wood grain, concrete, marble), skateboard decks, motorcycle seats, large textured surfaces where scale is ambiguous

**Why:** Session 119 -- product on newspaper stack looked 2x real size. Same on vinyl record. Skateboard and motorcycle worked because the surface is large enough that sunglasses naturally look small.

**How to apply:** When picking scenes for UGC_PRODUCT, use surfaces where the product can sit without competing size-reference objects nearby. Keep backgrounds simple -- Gemini fills naturally.
