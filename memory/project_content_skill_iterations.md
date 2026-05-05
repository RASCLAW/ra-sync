---
name: Content Skill Iterations -- v1 parked, v2 active
description: Brand + UGC skills are the active content path. v1 ad-creative/prompt-writer/validator/infographic-ad/ugc-fidelity-gatekeeper are parked, do not patch in place.
type: project
related: [feedback_naturalism_prompting.md, feedback_creative_range.md, feedback_brand_skill_rewrite.md, feedback_ugc_framing.md, feedback_ra_aesthetic_preference.md, project_brand_pipeline.md, project_messenger_strategy.md, project_v2_skills_validated.md, reference_prompt_reviewer_skill.md, feedback_use_skills_for_content.md, feedback_use_reviewer_before_gen.md, project_v3_fidelity_approach.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
DuberyMNL has two generations of content generation skills. Do not mix them.

**v1 (parked -- do not patch):**
- `dubery-ad-creative` -- first ad format writer
- `dubery-prompt-writer` -- WF2 caption-to-prompt writer
- `dubery-prompt-validator` -- validator enforcing v1 language
- `dubery-ugc-fidelity-gatekeeper` -- UGC validator enforcing v1 language
- `dubery-infographic-ad` -- hand-drawn callout ad format (still has P699 price baked in)

v1 characteristics: hard-fidelity language (`"MUST feature the exact style... Do not alter"`), fixed prompt templates, single reference example per format, single angle default. The validators literally REQUIRE the coercive phrase to exist — removing it from a writer breaks the validator chain.

**v2 (active path):**
- `dubery-brand-bold` -- rewritten 2026-04-09, full variety banks + WF2 banned-word fidelity + angle randomization
- `dubery-brand-callout` -- pending A2 rewrite to v2 pattern
- `dubery-brand-collection` -- pending A3 rewrite
- `dubery-ugc-prompt-writer` -- naturalism-first, needs variety banks (A4)

v2 characteristics: naturalism-first product instruction, variety banks per layout (scene/surface/lighting/treatment), build-fresh prompts from rules not templates, angle randomization across batches.

**Why:** RA spent many sessions iterating. The v2 pattern (brand-bold) works better — naturalism language + banned-word fidelity produces naturally-integrated products instead of pasted composites. But the v1 skills still work end-to-end because their internal logic is consistent (writer + validator enforce the same coercive language together). Patching one side of v1 breaks the chain.

**How to apply:**
- For **live organic + ad content**, use v2 brand-* and UGC skills. RA currently uses brand content AS ads since brand + ads overlap in the engagement-first Messenger-pivot strategy.
- For v1 skills, **leave them alone**. If you find the old coercive language, do NOT "fix" it — it's load-bearing for the validator chain.
- When RA actually needs paid ads again, build a **v2 ad-creative from scratch** (dedicated research pass). Don't retrofit v1.

**Decision point triggered A1 revert (session 107):** attempted to patch `dubery-ad-creative` product.instruction to naturalism language. Discovered `dubery-prompt-validator` PF-4 check requires the verbatim v1 phrase. Reverted. Scope refocused to A2/A3/A4 only -- the v2 path.

**Session 107 update (2026-04-12):**
- A2/A3/A4 complete. `dubery-brand-callout`, `dubery-brand-collection`, `dubery-ugc-prompt-writer` all rewritten to v2 pattern. Committed as `6080ada`.
- v2 pattern VALIDATED via smoke test — RA direct confirmation on brand-collection A/B ("prompt was already used, this version is much better"). See `project_v2_skills_validated.md`.
- New `/dubery-prompt-reviewer` skill built as v2 quality gate. See `reference_prompt_reviewer_skill.md`.

**Session 112 update (2026-04-12):**
- A7.1 DONE: R6 framing rule + Framing Bank added to UGC skill. Banned whole-body/wide shots for person-anchor.
- A7.2 DONE: TEXTURE surface bank replaced with clean premium (marble, walnut, slate, leather, bamboo, acrylic, metal).
- Bank rebalancing DONE across all 4 v2 skills + 3 v1 skills. Gritty locations/surfaces/atmospheres swapped for clean premium. AESTHETIC DEFAULT note added.
- `-2`/`-multi` angle ban applied across all 7 content skills. See `feedback_angle_ban.md`.
- `generate_vertex.py` now defaults to `contents/new/YYYY-MM-DD_{name}.png`.
- Test batch results: 4/8 passed. Bandits Green has persistent ~50% fidelity. Narrow scenarios (CAFE_TABLE) produce repetitive outputs. See `feedback_narrow_scenarios.md`.
- Remaining: product fidelity scorecard, cross-session dedup, batch randomizer (Python tool), first real batch volume+cadence.
