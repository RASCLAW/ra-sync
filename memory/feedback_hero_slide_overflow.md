---
name: Hero Slide Overflow Clip
description: overflow:hidden required on each carousel slide when child images use transform:scale, otherwise scaled content bleeds into adjacent slides
type: feedback
related: [project_dubery_v3_landing.md, feedback_carousel_hardcoded_widths.md]
originSessionId: 00396973-bcb9-40f7-8cac-ed7cbcc42391
---
Add `overflow: hidden` to every `.hero-slide` (or equivalent carousel slide container) when any child element uses `transform: scale()`.

**Why:** CSS `transform: scale()` on an image expands it visually beyond its natural bounds. Without `overflow: hidden` on the slide container, the scaled pixels render into the adjacent slide's visible area, causing visible bleed even when the parent carousel has `overflow: hidden`.

**How to apply:** Any time a carousel slide contains a scaled image — set `overflow: hidden` on the slide itself, not just the outer carousel wrapper.
