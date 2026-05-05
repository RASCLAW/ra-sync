---
name: Read Code, Don't Screenshot to See
description: When I need to understand what an existing site/component does, read its HTML/CSS/JS -- don't screenshot it for my own sake
type: feedback
related:
  - feedback_diagnostic_depth.md
  - feedback_document_testing.md
  - feedback_simple_flow_beats_scroll_scrub.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
When an existing site or component is already in the repo, read its source (HTML/CSS/JS, component files, config) to understand its structure, animations, design system. Don't spin up a server + Playwright screenshots just to "see it" myself.

**Why:** Screenshots take cycles (serve → tunnel → shoot → read PNG) and burn context. Reading code gives exact animation parameters, timing values, selectors, and design tokens directly -- more useful than pixels.

**How to apply:**
- Reference sites in the repo (e.g. `rasta-scroll-test/`, `dubery-landing/`) — read `index.html`, `css/*`, `js/*` to understand patterns.
- Screenshots are for *proving to RA* what the current build looks like (visual reviews, bug reports, "does this land"), not for me to orient to a codebase.
- Rule of thumb: if I'd screenshot something just to orient myself, read it instead. If I'd screenshot to show RA a result, screenshot is fine.
