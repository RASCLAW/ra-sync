---
name: Carousel CSS Widths Are Hardcoded Per Slide Count
description: Hero carousel track/slide widths must be updated manually in CSS when adding slides; JS auto-counts but CSS does not
type: feedback
related: [feedback_hero_slide_overflow.md, project_dubery_v3_landing.md]
originSessionId: a367eb7b-1466-466e-ae71-72f7189c8f38
---
When adding a slide to the v3 hero carousel, the CSS is hardcoded for the slide count — it does NOT auto-update like the JS does.

Must update two values together:
- `.hero-slides-track { width: N*100% }` — e.g. 2 slides = `200%`, 3 slides = `300%`
- `.hero-slide { width: 100%/N }` — e.g. 2 slides = `50%`, 3 slides = `33.3333%`

The JS (`slideW = 100 / total`) already uses `querySelectorAll` to count slides dynamically, so no JS changes needed when adding slides.

**Why:** Discovered when adding slide 3 — the new slide image bled into slide 2's right half because the track was still sized for 2 slides.

**How to apply:** Any time a slide is added or removed from `dubery-landing-v3/index.html`, update both CSS values in `styles.css` before testing.
