---
name: Thumbnail Grid Use grid-auto-rows
description: For gallery thumbnail grids, skip `aspect-ratio` on cards — use `grid-auto-rows: Npx` + `height: 100%` on the card. aspect-ratio is unreliable when the grid is inside nested flex/grid layouts.
type: feedback
related:
  - feedback_grid_min_width_zero.md
  - reference_command_center.md
---
For responsive thumbnail gallery grids (`grid-template-columns: repeat(auto-fill, minmax(Npx, 1fr))`), don't use `aspect-ratio: 1/1` on the cards — use `grid-auto-rows: Npx` on the grid container instead, with `height: 100%` on the card and `object-fit: cover` on the img.

**Why:** Command Center Marketing tab thumbnails were rendering as tall stacked strips even though Playwright inspector showed `aspect-ratio: 1 / 1` on `.mkt-thumb` with `thumb_h: 102, thumb_w: 102`. Live browser showed them stacked with no row gap. `aspect-ratio` on grid items inside a nested flex column layout (`.mkt-middle` is a flex column containing `.mkt-picker-card` which contains `.mkt-thumb-grid`) can misbehave depending on how intrinsic sizing resolves. Fixed-height rows via `grid-auto-rows` always work.

**How to apply:**
- Gallery thumb grid pattern:
  ```css
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
    grid-auto-rows: 110px;
    gap: 10px;
  }
  .card { width: 100%; height: 100%; overflow: hidden; }
  .card img { width: 100%; height: 100%; object-fit: cover; }
  ```
- For non-square thumbs, just set `grid-auto-rows` to the desired row height (it's independent of column width since `minmax(..., 1fr)` lets columns stretch).
- Don't mix `aspect-ratio` + `grid-auto-rows` — pick one, prefer `grid-auto-rows` for thumbnail grids.
- For Content Gen-style variable-height output grids, `aspect-ratio` may still be fine — this rule applies specifically to *fixed thumbnail* galleries inside nested layouts.
