---
name: DuberyMNL v3 Landing (Knockaround Light Theme)
description: dubery-landing-v3/ — light-theme Knockaround-inspired static site, vanilla HTML/CSS/JS, /order multi-variant picker + /products PDP + Apps Script order webhook wired
type: project
related:
  - project_dubery_landing_v2.md
  - reference_dubery_orders_sheet.md
  - project_product_catalog.md
  - project_portfolio_rebuild.md
  - reference_catalog_hover_workflow.md
  - project_v3_pdp_cart_redesign.md
  - feedback_homepage_base64_images.md
originSessionId: 70bad458-80be-401f-b689-c0aedf9cdc10
---
**Location:** `C:\Users\RAS\projects\DuberyMNL\dubery-landing-v3\`
**Preview:** `http://127.0.0.1:8094/` (python -m http.server 8094)
**Production:** NOT deployed. `dubery-landing/` still serves `duberymnl.com`. v2 (dark cinematic) and v3 (light Knockaround) are parallel sandboxes.

**Stack:** Static HTML + vanilla CSS + vanilla JS. No framework, no build step. Space Grotesk + Inter via Google Fonts. Light theme, white bg, black type, red accent.

**Pages wired:**
- `/` — home (hero + series + best sellers + shop-social + story + CTA)
- `/products/` — catalog grid with series filter (`?series=bandits|outback|rasta`)
- `/products/item.html?slug=<slug>` — PDP with gallery/qty/specs/order form
- `/order/` — multi-variant picker with qty controls + bundle logic; accepts `?model=<slug>&qty=<n>` prefill from PDP's "2 pairs" pill
- `/shop-social/` — placeholder feed grid

**Order flow (CRITICAL — plumbing validated 2026-04-19):**
- Both PDP and /order/ POST to `APPS_SCRIPT_URL` (see `reference_dubery_orders_sheet.md`)
- Payload shape: FormData with single `payload` field = JSON string of `{name, phone, address, notes, items, caption_id, grand_total, delivery_fee, express}`
- `items` = `[{name: "Bandits – Tortoise", qty: 1}, ...]` grouped per variant, uses EN-DASH (U+2013)
- Variant order names live in `dubery-landing-v3/products/data.json` as `order_name` field per variant
- Pricing: 1 pair ₱599 + ₱99 delivery = ₱698. 2+ pairs = ₱99 bundle discount + FREE Metro Manila delivery.

**PDP "2 pairs" pill behavior:**
- q=1: stay on PDP, single-variant checkout, delivery ₱99, total ₱698
- q=2: redirect to `/order/?model=<slug>&qty=1` so user can mix in a second colorway (PDP doesn't support multi-variant checkout)

**Hero carousel (added session 151):** 2-slide swipeable carousel. Arrows + dots + touch swipe. No auto-rotate.
- Slide 1: `outback-blue-hero.png` — copy left-aligned. Desktop: `object-position: 49% 44%`. Mobile: `object-position: 57% 28%; transform: scale(1.25)`.
- Slide 2: `outback-black-laughwall.png` — copy right-aligned (desktop), right-aligned full-bleed (mobile). Desktop: `object-position: 100% 30%; transform: scale(1.10); transform-origin: 100% 30%`.
- `hero-edit.js` has Slide dropdown (defaults to Slide 2). Exposes `window._heroGoTo` from carousel IIFE.
- Adding a 3rd slide: add `.hero-slide` to `#heroTrack`, update `.hero-slides-track { width: 300%; }` and `.hero-slide { width: 33.33%; }`, add a dot, update `slideW` calc. See `feedback_hero_slide_overflow.md` — always add `overflow:hidden` to new slides.
- Don't embed hero as base64 via editor.js — always extract to file (index.html ballooned to 28MB with base64).

**Vercel preview (2026-04-20):** deployed at https://dubery-landing-v3-58vvxvhnv-rasclaws-projects.vercel.app — use `vercel --yes` from `dubery-landing-v3/` to redeploy.

**Visual editors:**
- `dubery-landing-v3/hero-edit.js` — hero image crop editor; X/Y/Zoom sliders + Copy CSS; see `reference_hero_edit_js.md`
- `dubery-landing-v3/editor.js` — home page `?edit` mode; click images to replace, click text to edit, Save HTML bakes changes in
- `dubery-landing-v3/products/item-editor.js` — PDP editor, `?slug=X&edit`; replace/add gallery images, edit copy/specs
- `dubery-landing-v3/products/catalog-editor.js` — catalog editor, `/products/?edit`; toggle Hover/Primary view, replace images
- Best sellers trimmed to 4: Outback Black, Outback Blue, Rasta Red, Bandits Tortoise
- Filter pills (.bs-filters) and swatch dots (.bs-swatches) removed from best sellers

**Extract workflow (apply editor changes to live site):**
1. Save HTML from editor
2. Run inline Python script: regex-parse saved HTML for data: URL images, base64-decode + save to `assets/catalog/`, update `data.json`
3. Pattern confirmed working for: outback-black PDP (5 gallery images) + all 11 product hover images
4. Naming: `{slug}-gallery-{n}.png` for PDP, `{slug}-hover.png` for catalog hover

**Local preview:** `powershell Start-Process python -ArgumentList '-m http.server 8300' -WorkingDirectory 'C:\Users\RAS\projects\DuberyMNL\dubery-landing-v3'`
Then: http://localhost:8300 (normal) or http://localhost:8300?edit (editor)

**Remaining punch list:**
- Real hover/gallery shots per variant
- Real testimonials (current ones are placeholders)
- Real UGC in /shop-social/
- OG / Twitter meta tags
- Mobile nav drawer verification
- Deploy to Vercel preview
- Domain swap v1 → v3

**Validated end-to-end 2026-04-19:** Two test orders landed in "DuberyMNL Orders" sheet (row 9 smoke bandits-tortoise + outback-red ₱1099; row 10 RA's real test 3 pairs ₱1698). Math: n×599 − 99 bundle discount = grand total for n≥2.
