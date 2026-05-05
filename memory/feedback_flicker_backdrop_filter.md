---
name: Backdrop-Filter Over Scroll-Animated Backgrounds = Flicker
description: Never use backdrop-filter on cards/elements when a scroll-scrubbed animated visual is behind them -- GPU re-rasters every frame
type: feedback
related:
  - feedback_simple_flow_beats_scroll_scrub.md
  - project_dubery_landing_v2.md
  - feedback_cloudflare_edge_cache_bust.md
  - feedback_section_coverage_flicker.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
Rule: if a page has a scroll-linked animated visual layer behind the content (e.g. peacock tile-floor with `yPercent: -50 scrub:true` on 262 img tags), do NOT apply `backdrop-filter: blur(...)` to any foreground card or hover element. The browser re-rasters the blurred region every scroll frame, causing visible flicker — worst on wide-viewport monitors during fast scroll.

**Why:** Session 129 best-sellers flicker. Two stale `.featured-card` + `.series-media` rule blocks from the pre-"no chrome" CSS had `backdrop-filter: blur(8px)` that the later declarations didn't override. Blur over the 262-img perspective-tilted peacock grid forced the GPU to recompute the blurred bg on every `ScrollTrigger.update`. Deleting the duplicates killed the flicker instantly.

**How to apply:**
- Audit for `backdrop-filter` in any stylesheet that sits in front of a scroll-animated bg. If found, remove or replace with solid `background: rgba(...)` (cheap — no GPU cost).
- When refactoring card chrome → "no chrome", **delete the old rule block rather than try to override it**. Overriding leaves properties like `backdrop-filter` and `background` intact if the new rule doesn't list them explicitly. CSS-cascade override-patching is a source of subtle flicker bugs.
- On foreground cards that DO have hover transforms (scale / translate): add `transform: translateZ(0)` + `will-change: transform` to force GPU compositor promotion. Prevents layer rebuilds when a hover transition fires mid-scroll.
- Debug flicker via Playwright DOM inspection + check `getComputedStyle(el).backdropFilter` rather than screenshots — the flicker itself doesn't show in screenshots.
