---
name: Brand Pipeline Research + duberymnl.com v2 Direction
description: Session 122 research into Knockaround-inspired brand content + site redesign. Full report + backlog items.
type: project
related: [project_brand_pipeline.md, project_content_skill_iterations.md, feedback_holding_product_forward.md, feedback_ra_aesthetic_preference.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
Session 122 research synthesized competitive DTC brand scan (Knockaround, Goodr, Sunski, Pit Viper, Warby Parker, DIFF, Oakley) + direct HTML/CSS inspection of knockaround.com and duberymnl.com. Full report at `c:/Users/RAS/projects/DuberyMNL/.tmp/brand-research-report.html`.

**Direction locked (RA-confirmed):**
- Quiet confidence + active lifestyle + premium feel
- NO self-deprecating humor, NO gritty/commuter scenes
- Photography-first for discovery surfaces (no text overlays on tiles)
- Typography formats (BOLD/CALLOUT/COLLECTION) reserved for ads/promos
- Proposed content mix: ~40% lifestyle photography + 20% model shots + 25% typography + 15% specialty

**Backlog entries (in current-priorities.md):**
1. Brand content pipeline expansion: /brand-pipeline orchestrator + brand_randomizer.py + new skills (LIFESTYLE_TILE, UGC_STYLE) + format categories (TESTIMONIAL_SCREENSHOT, PRICE_VS_LUXURY, PRODUCT_NAMED_POSTER, ACTIVE_LIFESTYLE)
2. duberymnl.com v2 Knockaround-inspired port: trust strip + promo bar + media-card tile grid + best-sellers + shop-IG wall + founder story. Path A (enhance existing static site) recommended.

**Rejected formats:**
- GLARE_POV — doesn't fit quiet-confidence direction
- OCCUPATION_IN_ACTION (original framing with worker/fisherman contexts) — reframed to ACTIVE_LIFESTYLE (surfer/cyclist/hiker/weekend warrior)
- Irreverent humor headline theme — replaced with Quiet Confidence

**Key technical findings:**
- Knockaround runs Shopify + custom theme (Dawn-influenced) + Tailwind + BEM + web components. Shop-IG wall is Pixlee ($200+/mo).
- duberymnl.com is static HTML + vanilla CSS + JS (392 lines HTML, 2078 lines CSS). Already has Barlow Condensed + Inter, dark mode toggle, hero + polarized proof + social strip. Strong bones.
- Build path: enhance existing static site, don't migrate to Shopify (would break Messenger-first funnel). Use Curator.io (~$35/mo) or custom IG Graph API for shop-IG wall.
- ~8-12 hrs total dev time estimated for full port.

**Dependencies:**
- LIFESTYLE_TILE skill must be built first (feeds tile-grid images before v2 launch)
- Need founder story copy written (150-250 words, quiet-confidence tone)
- IG posting cadence needs to start before shop-IG wall gets meaningful content

**Toolkit for v2 hero (from 2026-04-17 Jono Catliff ingest):**
- Three.js for subtle 3D accent (free, no API) — `threejs.org/examples` has ~153 examples
- Spline for 3D graphic blocks (free, use black→transparent gradient to hide "Built with Spline" watermark)
- Seedance 2.0 for cinematic looped bg video — **use kie.ai direct via existing key** (NOT Higgsfield, which is $15/mo paid wrapper). Nate Herk's `nate-seedance-websites.md` has the kie.ai prompt pattern.
- Kling 3.0 before/after transition videos — skip for DuberyMNL, reserve for RAS Creative solar installer demos.
- Deploy stays as existing static → Vercel. No Next.js migration needed unless using Three.js React wrapper.
- See `summaries/jono-4-tools-10x-claude-websites.md` for the full pattern.

**Carousel ad-format reference (2026-04-19 Glowwave ingest):**
- `reference_glowwave_carousel_design.md` — 4 unity constants + 6 layout formats + per-slide mood wash for the typography/ad carousels called out in the backlog.
- Gives concrete specs to 3 of the outstanding new-skill ideas: HERO_PORTRAIT (new), UGC_WALL ≈ TESTIMONIAL_SCREENSHOT (new), MULTI_PRODUCT_GRID with neon tags (variant on existing `dubery-brand-collection`).
- Tone caveat: take the structure, not the loud palette/"!" copy — keeps the quiet-confidence voice from `project_dubery_v2_brand_identity.md` intact.
- Full summary: `~/projects/EA-brain/references/summaries/behance-glowwave-carousel.md`.
