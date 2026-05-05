---
name: /dubery-prompt-reviewer skill
description: v2 content quality gate built session 107. Reviews prompt JSONs from brand-bold/brand-callout/brand-collection/UGC against V1-V7 universal + per-skill checks. Returns PASS/PATCH/FAIL. Use before image gen spend.
type: reference
related: [project_content_skill_iterations.md, project_v2_skills_validated.md, feedback_brand_skill_rewrite.md, feedback_use_skills_for_content.md, feedback_use_reviewer_before_gen.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
**Skill location:** `c:/Users/RAS/projects/DuberyMNL/.claude/skills/dubery-prompt-reviewer/SKILL.md`

**Applies to v2 skills only:**
- `dubery-brand-bold`
- `dubery-brand-callout`
- `dubery-brand-collection`
- `dubery-ugc-prompt-writer`

Does NOT apply to v1 skills (ad-creative, prompt-writer, infographic-ad, prompt-validator, ugc-fidelity-gatekeeper). Those have their own validator chain — see `project_content_skill_iterations.md`.

**Universal checks (V1-V7):**

| Check | What it verifies |
|---|---|
| V1 — R2 banned words | Product color/material/lens descriptors not in prompt text (context-aware: scene lighting using "amber" is OK, lens described as "amber" is not) |
| V2 — R3 render_notes | 5-field template present in order: POSITION / ANGLE / LIGHTING / LOGO / REFERENCE (N/A for UGC) |
| V3 — R4 lens reflection | No specific reflected content. Allowed: "lenses naturally catching the light and environment of the scene" |
| V4 — Variety bank usage | Specific bank picks evident in prompt text, not generic fallbacks ("a wooden table" = fail, "dark walnut wooden table" = pass) |
| V5 — Angle randomization | Not defaulting to `-1.png`. Collection: all products share same angle suffix (L2) |
| V6 — Prompt length | 3-10 sentences (UGC: 3-5). Not a wall of text, not under-specified |
| V7 — Structural integrity | Valid JSON, task field matches, image_input paths exist, api_parameters complete, forward slashes |

**Skill-specific checks** layered on top of universal:
- brand-bold: headline 3-5 words, ONE pair in prompt, max 2 text elements, no sales language
- brand-callout: 4-6 callouts present, 4-5 word labels, feature bank usage
- brand-collection: 2-4 products (odd preferred), same angle across products, ONE pair each
- UGC: naturalism opening, finish stated (glossy/matte), specific PH location, physical realism

**Verdicts:**
- ✅ **PASS** — ready for image generation
- ⚠️ **PATCH** — regenerate prompt via source skill, concept sound, minor issues to fix
- ❌ **FAIL** — skill-level bug (missing bank option, broken schema, bad rule), fix skill first, do NOT blindly regenerate

**Usage:**
- Single file: `contents/new/BOLD-001_prompt.json`
- Folder: `contents/new/` (reviews all `*_prompt.json` inside)
- Batch: comma-separated paths
- Output: per-file PASS/PATCH/FAIL report + batch summary

**When to invoke:** after generating prompt JSONs via any v2 skill, BEFORE running `tools/image_gen/generate_vertex.py`. Reviewer verdict gates the Vertex AI spend (~$0.07/image).

**Session 107 first use (2026-04-12):**
- Batch of 4 samples (bold, callout, collection, UGC)
- Result: 2 PASS + 2 PATCH
- Correctly flagged UGC R3 violation (`"lenses reflecting"` — prohibited phrase) and collection angle randomization note
- Fixes: 1-word UGC patch applied, collection note deferred to next-batch guidance
- All 4 went to image gen, 3 produced usable output, 1 (brand-bold TEXTURE) surfaced a skill-level aesthetic bias (separate issue, see `feedback_ra_aesthetic_preference.md`)

**Limitations (not caught by reviewer):**
- Aesthetic quality — only the generated image reveals this
- Caption alignment (UGC) — human judgment call
- Cross-session prompt combo repetition — reviewer has no memory of previous batches (backlog item)
- Product model accuracy — can't verify from text, only from rendered image
