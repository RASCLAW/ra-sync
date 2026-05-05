---
name: Brand Content Pipeline
description: Brand content skills -- 3 proven (callout, bold, collection), 3 pending (lifestyle, comparison, educational). Also serves as the Creative Engine layer of the RAS Creative SOLUTIONS stack.
type: project
related: [feedback_brand_skill_rewrite.md, feedback_carousel_format.md, feedback_creative_range.md, feedback_naturalism_prompting.md, project_refactor_recovery_session99.md, project_positioning_locked.md, project_content_skill_iterations.md, project_v2_skills_validated.md, feedback_ra_aesthetic_preference.md, project_brand_collection_formula.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---

**Positioning link 2026-04-11 (session 104):** These skills are the Creative Engine layer of the RAS Creative SOLUTIONS offer — the "fresh creative that never runs out" part of the locked positioning statement. Maps to Jordan Platten's System 3 (Creative Goldmine). See `project_positioning_locked.md`.

Brand content pipeline for DuberyMNL organic feed posts. Split into individual skills per scenario type.

**Status:** 3 skills production-tested, 3 scenarios parked.

**Proven Skills (Session 88):**
- `/dubery-brand-callout` -- 5 layouts: RADIAL, SPLIT, EXPLODED, NUMBERED, TOP_BOTTOM. All passing.
- `/dubery-brand-bold` -- 4 layouts: TYPE_COLLAGE, TEXTURE, SPLIT_TEXT, KNOCKOUT. All passing.
- `/dubery-brand-collection` -- 5 layouts: FLAT_LAY, HERO_CAST, DIAGONAL, FAN_SPREAD, UNBOX_FLATLAY. GRID dropped (looked pasted).

**Parked Scenarios:**
- LIFESTYLE_CARD: Works single-frame, needs own skill when tested more
- COMPARISON: Article companion only, POV through-lens concept works
- EDUCATIONAL: Article companion only, too noisy for single image

**Orchestrator:** `/dubery-brand-content` routes to sub-skills (needs update to reflect new structure)

**Key Learnings Baked Into All Skills:**
- Real environments mandatory (not plain backgrounds)
- Single angle reference per product (multi-angle causes ghost pairs)
- "ONE pair" instruction prevents duplicates
- Lens must reflect real environment
- Odd numbers for collections (3 or 5)
- Grid layout = dead (looks pasted)
- No test card in unboxing flatlay
- Wide 2:1 format for carousels, product-anchor only

**Why:** Brand awareness + education, 2x/week organic feed. Complements UGC (daily) and Ad Creative (engagement).

**How to apply:** Call the specific skill directly, or use `/dubery-brand-content` as router. Generate via `generate_vertex.py`.
