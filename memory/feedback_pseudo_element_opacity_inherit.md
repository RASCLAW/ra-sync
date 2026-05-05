---
name: Pseudo-element Opacity Inheritance Breaks Section Fade
description: ::before on flow-sections inherits parent opacity -- fights the JS fade system and causes double-dim flicker
type: feedback
related: [feedback_section_coverage_flicker.md, feedback_flicker_backdrop_filter.md]
originSessionId: 6e1fce92-1052-47da-9764-2e7485ec7e79
---
Never use `::before` or `::after` pseudo-elements on `.flow-section` for dimming overlays.

**Why:** Pseudo-elements inherit their parent's `opacity`. When a section fades in (opacity 0→1 via the rAF scroll listener), the `::before` dim overlay also animates from 0→1 simultaneously. Combined with the `#dark-overlay` JS system also ramping up, you get two competing dim animations that produce visible flicker — especially noticeable on mobile.

**How to apply:** All peacock dimming must go through `#dark-overlay` (position: fixed, z-index: 2, driven by `OVERLAY_MAX` in `script.js`). The overlay is independent of section opacity so it doesn't double-animate. Never add per-section background overlays via pseudo-elements.
