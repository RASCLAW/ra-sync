---
name: DuberyMNL v2 Website (Cinematic Dark + Peacock)
description: dubery-landing-v2/ — dark cinematic site with fixed peacock UGC tile-floor bg, simple flow sections, /products catalog. Preview via tunnel; NOT yet committed or deployed
type: project
related:
  - project_dubery_v3_landing.md
  - project_brand_pipeline_research.md
  - project_portfolio_rebuild.md
  - project_product_catalog.md
  - feedback_simple_flow_beats_scroll_scrub.md
  - reference_cloudflare_tunnel_preview.md
  - feedback_flicker_backdrop_filter.md
  - reference_dubery_font.md
  - feedback_font_build_pipeline.md
  - reference_visual_editor.md
  - feedback_transforms_break_clicks.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
**Location:** `C:\Users\RAS\projects\DuberyMNL\dubery-landing-v2\`
**Preview:** `https://review.duberymnl.com/` (served via `python -m http.server 8123` in sandbox + named Cloudflare tunnel)
**Production:** NOT deployed. `dubery-landing/` still serves `duberymnl.com`. This v2 is a sandbox until RA signs off on polish + swap.
**Git:** NOT committed. Lives only on laptop + tunnel-served. No `.vercel` project.

**Architecture:**

- **Stack:** Static HTML + vanilla CSS + vanilla JS. Lenis (smooth scroll) + GSAP 3 + ScrollTrigger via CDN. **Dubery** (custom, italic wordmark) + Space Grotesk (display fallback) + Inter (body) fonts. See `reference_dubery_font.md`.
- **Palette:** Dark cinematic. `--bg-dark: #0a0a0a`, `--text-primary: #f4f1eb`, `--accent-red: #E8110F`. No light-mode fallback.
- **Layout:** Normal document flow sections (no scroll-container wrapper, no absolute positioning, no opacity:0 reveals). Each section `min-height: 80vh` in natural flow.
- **Flex visual:** Fixed `.peacock-bg` tilted UGC tile-floor (131 UGC person + brand editorial images, 2× for seamless loop = 262 img tags). `gsap.to(grid, yPercent: -50, scrub: true)` bound to `document.body` — rolls with scroll pixel 1 → end.
- **UI rule:** No card chrome. Product cards, series cards, featured cards have no bg / no border. Just images + type on transparent layers so peacock tiles peek between elements.

**Section flow (home, 6 sections + hero):**
1. Hero standalone (DUBERY MNL wordmark + tagline + scroll indicator)
2. 001 / Protection — short copy blurb
3. 002 / Collections — 3 series cards linking to `/products?series=X`
4. 003 / Stats — animated counters (400nm UV / 100% polarized / 22g) via IntersectionObserver
5. 004 / Best Sellers — 5 featured product cards
6. 005 / Value — "Premium Without the Markup" copy + model shot
7. 006 / Get Yours CTA → m.me/duberymnl

**Products page:** `/products/` — 11 variant cards correctly mapped to `contents/assets/product-specs.json` (5 Bandits / 4 Outback / 2 Rasta). Series filter tabs (URL-synced via `?series=`). Deep-link anchors (`#bandits-matte-black` etc.) so home's best-sellers row jumps to specific products.

**Assets mapping:**
- Peacock tile-floor pool: `assets/tile-mix/tile-001..131.jpg` — UGC wearing + brand editorial, NO kraft product shots (see `feedback_kraft_not_in_ambient_bg.md`)
- Product hero shots: `assets/products/{variant}.jpg` — 11 variants from `contents/assets/hero/hero-*.png` thumbnailed to 800×800 JPG
- Legacy: `assets/ugc/` and `assets/products/` earlier copies (pre-pivot) remain but are superseded

**Iteration history during session 129:** 5 visual pivots landed on current dark-cinematic + simple-flow approach. Earlier explorations (light Knockaround, GSAP scroll-scrub flythrough, CSS rolling-credits, dark-glass full-page overlay) are archived in `.tmp/v2-archive/` — don't re-attempt without re-reading the feedback rationale.

**Session 132 updates:**
- Hero: two-tone DUBERY (off-white) + MNL (red), centered, red glow, logo-header.png above (rounded corners), 124px font
- Promo/util bars removed
- Protection section: grid layout -- text left + 3 product images right
- Value section: grid layout -- text left + 2 product flatlays side-by-side right
- Collection cards: model wearing shots (not product boxes)
- Peacock: 62deg lean, 0.55 opacity, dimmed vignette
- Section labels 002-005 removed
- Sections 100vh, fade-on-scroll effect, smooth Lenis (no snap)
- Visual editor built (`editor.js`, `?edit` URL param) -- see `reference_visual_editor.md`
- Light theme tested and reverted (too bright)
- Lightning effect built and removed

**Known open items:**
- Mobile responsiveness not tested
- Best Sellers section needs polish
- CTA section needs work
- Server keeps dying between edits (python http.server)
- Commit + deploy decision pending RA approval
- No /about, /how-it-works, /faq pages yet

**Related:** `reference_visual_editor.md`, `feedback_transforms_break_clicks.md`
