---
name: Section Min-Height Must Cover Viewport (Peacock Flicker)
description: flow-section min-height must be >= viewport height or moving peacock bg shows through gaps and flickers
type: feedback
related:
  - feedback_flicker_backdrop_filter.md
  - project_dubery_landing_v2.md
originSessionId: d2c13034-429e-4a95-916a-85832e0c9160
---
Set `min-height` below viewport height (e.g. `60vh`) on `.flow-section` elements causes visible flicker between sections on scroll.

**Why:** The peacock background is `position: fixed` with a GSAP `yPercent: -50 scrub` animation running continuously. When sections are shorter than the viewport, there are visible gaps between them. As you scroll, the moving peacock tiles show through those gaps — which looks like flickering because the tiles are always animating.

**How to apply:** Always keep `.flow-section { min-height: 90vh; }` or higher. To reduce "too much space" feeling, reduce internal `padding` (e.g. `4vh 6vw`) rather than shrinking `min-height`. The visual spaciousness comes from padding, not height.
