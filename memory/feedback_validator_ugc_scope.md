---
name: Validator UGC-Only Scope
description: dubery-v3-validator applies to UGC content prompts (pipeline output), NOT to kraft prodref generation prompts (setup step).
type: feedback
related: project_v3_fidelity_approach.md, reference_kraft_prodref_workflow.md, project_hero_prodref_categories.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---

`dubery-v3-validator` validates **UGC content prompts only** -- the prompts used to generate social/ad content via the v3 pipeline (PERSON_WEARING, FLATLAY, UNBOXING, OUTFIT_MATCH, etc).

**Do NOT validate:**
- Kraft prodref generation prompts (the one-time setup that created `contents/assets/prodref-kraft/{product}/01-hero.png` etc.) -- those use supplier images as source, have different identity/direction logic, and are RA-reviewed per kraft shot
- Brand category prompts (CALLOUT, BOLD, COLLECTION) -- see `dubery-prompt-reviewer`
- Legacy v2 prompts

**Why:** Session 121 smoke test ran validator against a bandits-blue kraft prompt and returned 4 of 8 FAIL -- but the actual prompt was fine and the kraft PNG passed RA review. The failures were validator-gap false positives (custom multi-image prefix, color word "black" that was a structural identifier, etc). Kraft prodref prompts don't fit the UGC schema -- different purpose, different rules.

**How to apply:**
- Only run `/dubery-v3-validator` on prompts whose `image_input` points to `contents/assets/prodref-kraft/` or `contents/assets/hero/`
- If a prompt's `image_input` points to `references/supplier-images/`, it's a kraft generation step -- skip the validator
- If the prompt is for a brand category (text overlays, graphic composition), use `dubery-prompt-reviewer` instead
