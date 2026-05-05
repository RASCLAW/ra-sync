---
name: v2 Content Skill Rewrite -- Validated ROI
description: v2 rewrite pattern (variety banks + WF2 fidelity + build-fresh) produces measurably better output than v1. Direct RA confirmation on collection A/B, session 107.
type: project
related: [project_content_skill_iterations.md, feedback_brand_skill_rewrite.md, feedback_creative_range.md, feedback_naturalism_prompting.md, feedback_ugc_framing.md, feedback_ra_aesthetic_preference.md, reference_prompt_reviewer_skill.md, project_brand_pipeline.md, feedback_use_skills_for_content.md, feedback_use_reviewer_before_gen.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
The v2 content skill pattern is validated. RA direct confirmation session 107 (2026-04-12).

**Evidence (session 107 smoke test):**
- Same prompt (Outback series HERO_CAST brand-collection) had been run before in a prior session with the v1 brand-collection skill
- v2 rewrite produced a new output that RA described verbatim: *"prompt was already used, this version is much better, reflection and product fidelity is top notch, can be used as ads or UGC"*
- All 4 v2 samples (bold / callout / collection / UGC) hit variety bank picks verbatim in the generated images — scene/lighting/surface/subject were visibly driven by bank choices, not generic fallbacks
- `dubery-brand-callout` RADIAL sample received RA's highest rating: *"looks perfect"*

**The v2 pattern (template for all future content skills):**

1. **Variety banks per layout/scenario** — 4-8 options each for surface, lighting, environment, subject, treatment, background. Pick one per generation. Never copy a reference template.
2. **WF2 fidelity via banned-words list (R2)** — frame colors, lens descriptors, materials banned from prompt text. Reference image is the only authority. Naturalism language ("matching the style shown in the reference image") replaces coercive language ("MUST feature exact style... Do not alter").
3. **5-field render_notes template (R3)** — POSITION / ANGLE / LIGHTING / LOGO / REFERENCE. No freeform notes.
4. **Lens reflection rule (R4)** — no specific reflected content. One allowed phrase: "lenses naturally catching the light and environment of the scene."
5. **Angle randomization** — rotate product reference angles (-1 / -2 / -3 / -4 / -multi) across batches. No defaulting to -1.png.
6. **Build fresh, not copy** — prompt is assembled from rules + bank picks each generation. Never verbatim from a template.
7. **Batch diversity check** — pick combos unique within the current batch. Future: track across sessions (backlog).

**How to apply:**

- When building new content skills, use `dubery-brand-bold` / `dubery-brand-callout` / `dubery-brand-collection` (post-session-107) as the reference template
- When refining existing skills, port the 7 elements above as a unit — partial ports break the pattern
- Run outputs through `/dubery-prompt-reviewer` before image generation spend (quality gate, V1-V7 checks)
- v1 skills are parked per `project_content_skill_iterations.md` — do NOT retrofit, build new when paid ads resume

**Remaining v2 issues surfaced in session 107 (not blockers, refinement):**

- `dubery-brand-bold` TEXTURE layout surface bank is aesthetically biased toward gritty/weathered — violates RA clean premium preference. See `feedback_ra_aesthetic_preference.md`. Next session: refine or retire.
- `dubery-ugc-prompt-writer` lacks a framing rule — whole-body wides allowed, product becomes unrecognizable. See `feedback_ugc_framing.md`. Next session: add R6 + tight-crop bank.
- Variety banks don't track cross-session history — 3 of 4 smoke test prompts were "already used" per RA. Backlog.
- Brand-callout scene bank is good enough to cross-pollinate to UGC as a "product-hero" variant (RA's insight). Backlog.

**Sessions implicated:** 2026-04-09 (brand-bold first v2 rewrite), 2026-04-12 session 107 (brand-callout + brand-collection + UGC variety banks + reviewer + smoke test validation).
