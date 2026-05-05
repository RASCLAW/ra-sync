---
name: Table min-width for mobile scroll
description: overflow-x-auto on wrapper alone causes column collapse on mobile; table needs explicit min-width
type: feedback
related: [project_team_faith_dashboard.md]
originSessionId: 590dce33-e1ff-43a6-ad42-807fe9b70a39
---
For a wide data table to scroll horizontally on mobile, the `<table>` element itself must have an explicit `min-width`. Without it, the browser tries to fit the table in the viewport by collapsing columns and wrapping cell content — the overflow-x-auto on the wrapper never triggers.

**Why:** Discovered on Team Faith Dashboard leaderboard — screenshot showed agent names wrapping onto 2–3 lines and columns still getting cut off, even with `overflow-x-auto` on the container div.

**How to apply:** Any time a table has more columns than fit on mobile:
```jsx
<div className="overflow-x-auto">
  <table style={{ minWidth: 860 }}> {/* or whatever fits all columns */}
```
Pick `min-width` wide enough to hold all columns without wrapping, but don't over-specify — the table will still shrink naturally on larger screens since it's `w-full`.
