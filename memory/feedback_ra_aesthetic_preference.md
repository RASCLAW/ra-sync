---
name: RA Aesthetic Preference -- Clean Premium
description: DuberyMNL content aesthetic is clean premium, not gritty/weathered/distressed. Applies to all brand + UGC content generation.
type: feedback
related: [feedback_ugc_quality.md, feedback_creative_range.md, feedback_ugc_framing.md, project_brand_pipeline.md, project_content_skill_iterations.md, project_v2_skills_validated.md, feedback_no_headband_outfit.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
RA prefers clean premium aesthetic across all DuberyMNL content. Gritty, weathered, rusted, distressed, grunge — all wrong for the brand.

**Why:** Session 107 smoke test (2026-04-12) generated `SAMPLE-BOLD-001` using `dubery-brand-bold` TEXTURE layout with variety bank picks Rusted metal door + wheat-paste poster text + Hanging from rusty nail + Golden hour light from the left. The output was technically correct — hit the bank picks verbatim — but RA rejected it: *"looks AI, nail thru product doesn't make sense, this prompt was already used, I also dont like the dirty and gritty scene."* The TEXTURE layout's surface+treatment bank was heavily weighted toward weathered aesthetics (rusted metal, mossy stone, corrugated steel, dark brick, wet asphalt, aged leather, weathered concrete). That bank is aesthetically biased against the brand.

DuberyMNL positioning = Filipino streetwear + lifestyle + premium affordable polarized sunglasses. Aesthetic coordinates:

- **Surfaces**: dark walnut, polished concrete (clean not cracked), aged brown leather (clean not worn), dark slate, brushed metal, dark marble, white marble, warm wooden tables, clean cafe surfaces
- **NOT**: rusted metal, mossy stone, wet asphalt, corrugated steel, weathered concrete, peeling paint, aged/worn anything, grunge textures
- **Lighting**: warm golden hour, clean studio, natural window light, soft overhead, dramatic side light (clean)
- **NOT**: neon noir, harsh dystopian, post-apocalyptic, low-key horror
- **Environments**: premium catalog, editorial studio, clean real-world (cafe, office, beach, home), modern urban
- **NOT**: abandoned buildings, industrial decay, dirty alleys, anything "street" in the distressed-graffiti sense

**How to apply:**

- When picking surface/environment bank options across brand-bold / brand-callout / brand-collection / UGC, prefer clean premium surfaces and reject gritty options
- **DONE (session 112):** brand-bold TEXTURE bank fully replaced with clean premium surfaces (polished marble, black slate, walnut panel, smooth concrete, leather panel, brushed metal, matte acrylic, bamboo). Product placement bank also cleaned (marble ledge, walnut shelf, leather tray, metal hook). All 4 v2 skills + 3 v1 skills rebalanced to remove gritty/weathered/rustic/reclaimed/worn language. AESTHETIC DEFAULT note added to UGC skill.
- When writing NEW variety banks, audit every option against this aesthetic rule before adding
- RA's brand is cleanly aspirational, not grunge-nostalgic
- Edge case: "real environments" per `project_brand_pipeline.md` is still the rule — plain backgrounds look CG. Real != gritty. A clean dark wooden cafe table is real AND premium.
