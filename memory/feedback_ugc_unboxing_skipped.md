---
name: UGC_UNBOXING -- Resolved, Back In Pipeline
description: RESOLVED session 121 -- UGC_UNBOXING is back in the pipeline and now uses the hero shot as prodref (no accessory hallucination). See project_hero_prodref_categories.md.
type: feedback
related: project_v3_fidelity_approach.md, feedback_fidelity_prefix.md, feedback_no_hardcoded_examples.md, project_hero_prodref_categories.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

**Status (session 121):** RESOLVED. UGC_UNBOXING is back in the v3 pipeline, using the hero shot at `contents/assets/hero/hero-{product}.png` as prodref. Tested live on outback-black UGC_DELIVERY (sibling hero category) -- all package components faithful. Kept this memory for historical context on the session-120 regression and the reasoning behind switching to hero prodref.

---

UGC_UNBOXING was temporarily out of scope during session 120. The category was validated in session 119 using the hero shot as prodref, but broke in session 120 when we switched to kraft prodref + explicit accessory text + strengthened DUBERY spelling guards.

**Why it fails now:**
- Strengthened branding line ("character-for-character" DUBERY spelling)
- CRITICAL spelling prefix in the mandatory prompt header
- Explicit accessory descriptions with "DUBERY" mentioned 3+ times
- Combined effect: Gemini over-renders DUBERY text on surfaces (cloth, box) and loses the metal temple badge

**Why blue worked in session 119:**
- Minimal accessory line: "Dubery branded box, drawstring pouch with red carabiner, microfiber cloth, and warranty card as shown in the reference image"
- Hero shot provided visual authority for accessories -- no need to describe them in text
- No CRITICAL spelling prefix at that time

**How to apply:** Do NOT generate UGC_UNBOXING until a cleaner approach is designed. Options: (1) create kraft versions of hero shots that include all accessories, (2) add a per-category prefix override that disables the CRITICAL spelling guard, (3) design an accessories-only image input alongside the kraft prodref. RA's direction: skip for now, revisit later.
