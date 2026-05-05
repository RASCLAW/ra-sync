---
name: Playwright fallback when WebFetch returns gallery metadata only
description: Behance/Dribbble/visual-gallery sites render images via JS — WebFetch sees only DOM metadata. Use a throwaway Playwright script to scroll + download the actual image assets.
type: reference
related: [reference_glowwave_carousel_design.md, reference_node_playwright_fallback.md, project_brand_pipeline_research.md]
originSessionId: a2b371a2-3f04-4652-adce-449a560f7697
---
When RA shares a Behance / Dribbble / Pinterest / other visual-gallery URL and you need to actually **see** the images, WebFetch will only return page metadata + tags (Behance specifically hides gallery image URLs behind a React app). Symptom: the WebFetch summary says things like "unfortunately, the actual visual designs aren't fully described."

**Fallback pattern:** write a ~40-line Playwright script in `.tmp/<slug>/scrape.js`:
1. `chromium.launch()` headless, viewport 1400x900
2. `page.goto(url, { waitUntil: 'networkidle' })`
3. Scroll loop: 10-12 wheel events + 700ms waits to trigger lazy-load
4. `$$eval('img', …)` filtering `naturalWidth >= 600` to drop thumbnails
5. Dedupe by URL (strip `?` query)
6. https.get() download each to `.tmp/<slug>/slide-NN.{ext}`
7. Read each downloaded file with the Read tool — Claude Code sees them as actual images

**Why:** Node + Playwright v1.59+ is already on RA's machine (`npx playwright --version`). No auth, no MCP, no paid wrapper needed. Total runtime ~30s for a typical 7-slide Behance carousel. Cached locally for later re-reads.

**When NOT to use:** static sites, news articles, blog posts — WebFetch handles those fine. Only reach for Playwright when the page is a JS-rendered gallery and WebFetch returned just metadata.

**Gotcha:** Behance sidebar recommends unrelated projects whose thumbnails also get scraped. Filter by image size (≥600px wide) AND cross-check slide count against the project page (visible in WebFetch metadata or page heading). Cover composite slide often appears first — that's the "hero" render, not a carousel slide; judge by content.

**Example:** Glowwave Behance ingest 2026-04-19 — 17 images downloaded, 7 were the real carousel, 5 were unrelated sidebar recommendations, 5 were thumbnails/footer logos. Images read inline via `Read` tool drove the full design-DNA extraction.
