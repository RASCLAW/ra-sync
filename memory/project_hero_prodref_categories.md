---
name: Hero Prodref for Package Categories
description: Package-centric UGC categories (UNBOXING, GIFTED, WHAT_YOU_GET, DELIVERY) use the hero shot at contents/assets/hero/hero-{product}.png as prodref, not a kraft prodref. Hero sidecars have no frame_direction.
type: project
related: reference_kraft_prodref_workflow.md, project_v3_fidelity_approach.md, feedback_ugc_unboxing_skipped.md, feedback_subject_placement_scene_only.md, reference_pipeline_skill_chain.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

Package-centric UGC categories anchor their prodref on the **hero shot** (full packaging flatlay: Dubery box + sunglasses + pouch + cleaning cloth + warranty card), not on a kraft prodref:

| Category | Prodref |
|----------|---------|
| `UGC_UNBOXING` | hero |
| `UGC_GIFTED` | hero |
| `UGC_WHAT_YOU_GET` | hero |
| `UGC_DELIVERY` | hero |

All other UGC categories (PRODUCT, PERSON_WEARING/HOLDING/SELFIE, OUTFIT_MATCH, FLATLAY) use kraft prodrefs from `contents/assets/prodref-kraft/{product}/`.

**Why:** The session-120 UNBOXING regression (DUBERY text smeared on cloth/box surfaces, accessories hallucinated) was caused by asking Gemini to invent the package from a kraft prodref + verbose accessory descriptions. The fix: feed Gemini the actual hero shot so box, pouch, cloth, and warranty card are all anchored by reference. Validated session 121 on outback-black UGC_DELIVERY -- all package components faithful.

**Hero sidecar rules:**
- Located at `contents/assets/hero/hero-{product}.json`
- NO `frame_direction` field (hero is overhead package layout, not a product angle)
- `visible_details` still applies (filters product-specs.json required_details)
- `shows` field lists what the hero image visually conveys (box, sunglasses, pouch, cloth, card)

**Randomizer + validator behavior:**
- Randomizer: `sidecar.get("frame_direction")` returns None for hero
- v3-validator V4: skips direction check when sidecar has no frame_direction; clock-direction ban still applies
- SKILL.md state templates for hero categories do NOT use `{frame_direction}` placeholder

**How to apply:** When adding a new package-centric UGC category, route it to hero prodref (not kraft). When a new product is added to the catalog, remember both kraft prodrefs (01-hero, 06-front, optional 07-flat) AND a hero shot are needed for full pipeline coverage.
