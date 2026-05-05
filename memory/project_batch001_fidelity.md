---
name: Batch 001 Product Fidelity Results
description: First real batch (11 images, 4 skills) scored 9 PASS / 1 FAIL / 1 ALMOST. Outback Blue and Bandits Blue are risky products.
type: project
related: [feedback_use_skills_for_content.md, feedback_use_reviewer_before_gen.md, project_content_skill_iterations.md, reference_product_refs.md, feedback_angle_ban.md]
originSessionId: aff172f2-5c57-4baa-b3b0-bc46b873440b
---
Batch 001 (session 114, 2026-04-13). 11 images generated via batch randomizer → v2 skills → prompt reviewer → generate_vertex.py.

**Results:**
- Outback Black (BOLD SPLIT_TEXT): PASS
- Bandits Tortoise (BOLD TYPE_COLLAGE): PASS
- Rasta Red (BOLD TEXTURE): PASS (looks a bit AI)
- Outback Green (CALLOUT RADIAL): PASS
- Bandits Green (CALLOUT TOP_BOTTOM): PASS
- Bandits diagonal collection (Blue+Green+GlossyBlk): PASS
- Bandits Matte Black (UGC PRODUCT_HOLD): PASS
- Outback Red (UGC SELFIE_OUTDOOR): PASS
- Rasta Brown (UGC PRODUCT_HOLD): PASS
- Bandits flat lay collection (Tortoise+MatteBlk+Blue): ALMOST -- Bandits Blue failed fidelity
- Outback Blue (UGC GYM_BAG): FAIL -- Dubery emblem altered

**Safe products (high confidence):** Outback Black, Outback Red, Outback Green, Bandits Matte Black, Bandits Tortoise, Bandits Green, Rasta Brown, Rasta Red, Bandits Glossy Black
**Risky products:** Outback Blue (emblem altered -- failed both scorecard rounds), Bandits Blue (fidelity issues in collections)

**RA also noted:**
- Headlines/text from the bank are being reused -- need cross-session dedup
- Callout layouts already used in previous sessions -- need layout history
- BOLD TEXTURE layout can look "a bit AI"

**How to apply:** When building batches, weight toward safe products. Use risky products sparingly and inspect output before posting. Outback Blue may need a better reference photo or specific prompt workaround.
