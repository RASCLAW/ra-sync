---
name: Transforms Break Click Targets
description: CSS transforms move visuals but click/hit areas stay at original position -- use padding/margin/grid for layout shifts instead
type: feedback
related:
  - project_dubery_landing_v2.md
  - feedback_simple_flow_beats_scroll_scrub.md
  - feedback_grid_min_width_zero.md
originSessionId: 0aaab2d6-0644-4d3d-bb2f-8639ee9a9d5e
---
Never use CSS `transform: translate()` for large layout shifts (>20px). The visual moves but the click target stays at the original DOM position, making elements unclickable where they appear.

**Why:** Discovered during dubery-v2 editor session. RA dragged elements with the visual editor which exported transforms. Applied as CSS transforms → elements looked right but couldn't be clicked/selected. Had to convert to padding/margin/grid.

**How to apply:** When the visual editor exports `transform: matrix(1,0,0,1, X, Y)`:
- Small offsets (<20px): transforms are fine
- Large Y offsets: convert to `padding-top` or `margin-top` on the section/container
- Large X offsets: convert to `padding-left` or grid column changes
- Multiple elements with same offset: adjust the parent container's layout instead
