---
name: Grid Item min-width 0 for Overflow
description: CSS grid items default to min-width auto (intrinsic content width). Wide children push tracks wider than their fr share. Set min-width 0 on the grid child to let it shrink + overflow-auto on inner container will scroll.
type: feedback
related:
  - feedback_transforms_break_clicks.md
  - feedback_thumbnail_grid_auto_rows.md
  - reference_command_center.md
originSessionId: 5d42280e-3225-4bd2-865c-4e2a10340ad9
---
CSS grid children default to `min-width: auto` — their minimum width is their intrinsic content width. Wide content (long rows of images, long strings) pushes the track wider than its `fr` share, ignoring `grid-template-columns`. Fix: set `min-width: 0` on the grid child. Any inner container with `overflow-x: auto` will then scroll inside the tracked width.

**Why:** Command Center Content Gen right column (7fr) kept getting wider as history images piled up, even though `.cg-history-area` already had `overflow-x: auto`. The scrollbar never appeared because `.cg-right` / `.cg-workspace` were growing with the content. Adding `min-width: 0` to both grid children clamped them to the 7fr track and the existing overflow kicked in.

**How to apply:**
- Any time a CSS grid track grows beyond its declared share when content gets wide → the grid child needs `min-width: 0`.
- Same gotcha exists on flexbox children (`min-width: auto` default). If an `overflow: auto` inside a flex/grid child isn't scrolling, check the parent's min-width.
- Safe default for any grid/flex child that contains horizontally-scrollable content: set `min-width: 0` up front.
