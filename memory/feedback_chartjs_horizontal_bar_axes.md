---
name: Chart.js Horizontal Bar xAxisID Rule
description: With indexAxis:'y', dataset axis refs must use xAxisID not yAxisID -- wrong ID causes silent 0-1 scale, invisible bars
type: feedback
related: [project_informdata_dashboard.md]
originSessionId: cd0c5b28-602b-4a3c-9233-e0de7a3a40a7
---
With `indexAxis: 'y'` (horizontal bar chart in Chart.js), value-axis datasets MUST reference `xAxisID`, not `yAxisID`. Using `yAxisID` causes a silent failure: the chart renders with a 0–1.0 default X-axis and all bars appear invisible while the category labels display correctly.

**Why:** Chart.js flips axis roles when `indexAxis:'y'` — category axis becomes Y (labels on left), value axis becomes X (numbers at bottom/top). Dataset axis ID properties must match this flipped convention. `yAxisID` is simply ignored and a default `x` scale from 0–1 is generated instead.

**How to apply:** Any time building a horizontal bar chart in Chart.js:
- All bar/line datasets: use `xAxisID: 'xSomeName'` (not `yAxisID`)
- In `scales`: define custom X axes with `position: 'bottom'` or `position: 'top'`
- The `y` scale key (no suffix) handles category labels only
- Test: if X-axis shows 0.0–1.0, this is the bug — check `xAxisID` vs `yAxisID` on datasets
