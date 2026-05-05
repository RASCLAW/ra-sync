---
name: Simple Flow Beats Complex Scroll-Scrub Visibility Systems
description: For scroll-driven sites, prefer normal document flow with always-visible sections over data-enter/leave/ScrollTrigger visibility dispatcher systems
type: feedback
related:
  - feedback_read_code_not_screenshot.md
  - project_dubery_landing_v2.md
  - feedback_flicker_backdrop_filter.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
When building a scroll-driven site, resist copying rasta-scroll-style section visibility systems (sections with `position: absolute; opacity: 0; data-enter="X"; data-leave="Y"` toggled via `ScrollTrigger.onUpdate`). Use normal document flow with sections always at opacity: 1.

**Why:** Attempted the rasta-scroll dispatcher three times during session 129. Sections disappeared mid-scroll, cuts happened at wrong moments, Lenis + Playwright interactions fought the ScrollTrigger timing, dead-zones between sections. RA specifically called it "glitchy" and asked for "simpler scroll behavior." Normal flow (each `<section>` with `min-height: 80vh`, naturally stacked) + a fixed scroll-linked background layer for the flex visual = rock solid.

**How to apply:**
- Keep ONE scroll-linked animation: the fixed visual bg (peacock tile-floor, canvas frames, video loop). Bind it to `document.body` scroll progress.
- Everything else: normal flow. Let the browser do what it does.
- Ditch `data-enter`/`data-leave`/`data-animation` attributes. Ditch `tl.play()`/`tl.reverse()` on scroll. Ditch `opacity: 0` + JS-toggled `.visible` class.
- Keep only: Lenis for smoothness, GSAP for the one bg scrub, progress bar, IntersectionObserver for one-shot counters, simple hero fade via scrollY/innerHeight.
- If a reveal animation is truly needed later, use IntersectionObserver + CSS transition — not a scroll-percentage dispatcher.
- Reference architecture: `dubery-landing-v2/` after session 129 simplification.
