---
name: Brand Collection Gen Formula (validated)
description: Codified formula for brand-collection images — 3 scene levers + fidelity triad + 5-input attachment pattern. Produces 100% product fidelity with RA-passing composition.
type: project
related: [project_brand_pipeline.md, feedback_color_free_specs.md, feedback_stripped_prompt_fields.md, feedback_use_skills_for_content.md, project_batch001_fidelity.md, reference_pipeline_skill_chain.md, feedback_prompt_bloat_fidelity.md, feedback_branding_hide_flatlay_only.md, project_rasta_red_prodref_issue.md]
originSessionId: brand-coll-batch-b3
---

Validated 2026-04-17 on batch B3 (4/4 RA-passing on first gen). Collection images with 100% fidelity + clean typography via a small, stable formula.

**Why:** UGC prompts fight scene narrative (coffee shop, beach, weather, props, mood) — Gemini burns capacity inventing + integrating, fidelity erodes. Brand-collection prompts give Gemini ONLY three scene levers, so model attention concentrates on the kraft product refs.

**How to apply:** When building a collection image prompt, follow this formula verbatim. Don't freelance prose. Don't add mood description, weather, time-of-day, props beyond packaging.

## The formula

### 5-input attachment pattern
```
INPUT_IMAGE_0 .. INPUT_IMAGE_{N-1}: one kraft prodref per product (contents/assets/prodref-kraft/{model}/01-hero.png)
INPUT_IMAGE_{N}: DUBERY-FONTS.png (font alphabet ref)
INPUT_IMAGE_{N+1}: dubery-logo.jpg (black bg for dark scenes) OR dubery-logo.png (white bg for light scenes)
```

For UNBOX variants: append dubery-packaging.png as a 6th ref.

### Three scene levers (the only creative variables)
1. **Surface** — one-line texture description. Premium studio-clean only. Batch B3 bank: dark bouclé fabric, terrazzo slab, brushed gunmetal, tadelakt plaster, charcoal wool felt, basalt slab, dark linen weave, matte navy ceramic tile, anodized aluminum, dark cork, brushed gunmetal, dark micro-suede.
2. **Lighting** — direction + quality + mood in one sentence. Single source. All products share it.
3. **Arrangement** — named layout (interlocking triangle, DUO mirror, horizontal lineup, DUO fanned, diagonal, HERO_CAST, exploded UNBOX).

### Fidelity triad (locked, never changes)
1. `blending_mode: PHOTOREALISTIC_INTEGRATION`
2. `relight_instruction`: "Use the products in INPUT_IMAGE_0 through INPUT_IMAGE_{N-1} but digitally relight all of them uniformly to match the scene's surface and light source, so they look physically present together on the same surface in the same moment. Identity, frame shape, finish, lens appearance, and pattern details of every product come entirely from the input reference images — not from text."
3. Per-product fidelity line: `Product in INPUT_IMAGE_N: {identity from product-specs.json}. {filtered required_details}. Proportions: {proportions}. Orientation: {frame_direction from sidecar}, matching the reference photo orientation.`

### Typography block
```
TYPOGRAPHY:
- Headline at the top of the frame: "{HEADLINE}" — bold italic sporty typeface from INPUT_IMAGE_{font_index}
- Tagline directly beneath, smaller: "{POLARIZED_TAGLINE}" — same typeface
- Identity line below products: "DUBERY {SERIES}" — single-series images only, skip for cross-series
- DUBERY logo bottom-right, small, from INPUT_IMAGE_{logo_index}
- Total text + logo under 15% of image
- NO sales language / prices / ORDER NOW / phone numbers / other text
```

**Polarized tagline rotation (3-cycle):** STAY POLARIZED, ALWAYS POLARIZED, DUBERY POLARIZED.

**Font color accent rule (session 127):** Match typography color to the dominant lens/arm accent of the products in frame. Rasta series → warm amber or red (matches stripe + lenses). Outback colors → match the lens mirror color (blue / green / red). Bandits tortoise → amber. Single-color stack → accent. Mixed frame → default to white on dark surfaces, dark charcoal on light surfaces. Tells Gemini explicitly so it picks the accent instead of defaulting to plain white/black.

### Color/finish word stripping (R2)
- Strip from text: blue, red, black, green, brown, amber, tortoise, camo, matte, glossy, dark, clear, tinted, mirrored, vibrant, warm (as lens), cool (as lens), gold, silver, smoke, honey, sapphire
- Model names allowed in product identity only (e.g. "Dubery Bandits Blue Polarized Sunglasses"), never as color cue
- Gemini reads color/finish/pattern from the kraft ref photos

## Build sequence per image

1. Load sidecar at `contents/assets/prodref-kraft/{model}/01-hero.json` → `frame_direction`, `visible_details` per product
2. Load `contents/assets/product-specs.json` → filter `required_details` by sidecar `visible_details` indices, strip color words
3. Pick scene lever values (surface + lighting + arrangement)
4. Pick headline + tagline (from rotation) + identity line (if single-series)
5. Write `.tmp/COLL-XXX_prompt.txt` with the formula blocks in order: PRODUCT FIDELITY → INTERACTION PHYSICS → SCENE → TYPOGRAPHY → RENDER
6. Write `.tmp/COLL-XXX_config.json` with image_input list + api_parameters (aspect_ratio `4:5`, resolution `1K`, output_format `png`)
7. `python tools/image_gen/generate_vertex.py .tmp/COLL-XXX_prompt.txt contents/new/COLL-XXX.png`
8. RA scores; iterate if FAIL, max 2 regens before discussion

## Two-pass identity text pattern

Sometimes the first gen lands composition but omits an identity text line. Instead of regen, run a lightweight image-to-image edit pass:

```
Take INPUT_IMAGE_0 and add the text "DUBERY {SERIES}" centered below the sunglasses, above the DUBERY logo. Use the same bold italic sporty typeface as the existing headline. Keep every other element of the image unchanged.
```

Config: `image_input: ["contents/new/COLL-XXX.png"]`. Cheap (~$0.10), preserves composition, adds missing text cleanly. Used on 001, 002, 003 retrofit in batch B3.

## What NOT to include

- Lens reflection descriptions (no "palm trees reflected", no "skyline in lens")
- Prop narratives (no "magazine on the table", "coffee cup nearby")
- Weather / time-of-day / location
- Mood adjectives beyond the lighting sentence
- Multiple light sources
- Multiple layout ideas in one prompt

## Path to pipeline skill

This formula is the spec for a future `/brand-collection-pipeline` skill (mirroring /ugc-pipeline's randomizer → fidelity-prompt → validator → generate chain). Variables to randomize: surface (from bank), lighting (from bank), arrangement (from bank). Fixed: fidelity triad + typography structure + 5-input pattern. Build when batch B3 completes + validates the surface/lighting/arrangement bank at scale.
