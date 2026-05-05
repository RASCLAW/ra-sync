---
name: touch-action pan-x for mobile table scroll
description: overflow-hidden parent intercepts touch on mobile; add touch-action:pan-x to the overflow-x-auto child to claim horizontal swipes
type: feedback
related: [feedback_table_min_width_mobile.md, project_team_faith_dashboard.md]
originSessionId: d65f3e7b-4fe9-428d-9a61-568348898d04
---
When a card/container has `overflow: hidden` (common for rounded-corner clipping) and a child has `overflow-x: auto`, mobile browsers (especially iOS Safari) assign touch gestures to the parent — which can't scroll. The child's scroll area is never reached by swipe.

**Why:** Discovered on Team Faith Dashboard — programmatic `scrollLeft = 300` worked in Playwright, but real touch/swipe did not. Playwright audit of `getComputedStyle(wrapper).touchAction` returned `"auto"`, confirming the browser had not assigned scroll ownership to the child.

**How to apply:** On every `overflow-x-auto` div that sits inside an `overflow-hidden` parent, add:
```jsx
<div className="overflow-x-auto" style={{ touchAction: 'pan-x pan-y', WebkitOverflowScrolling: 'touch' }}>
```
- `touch-action: pan-x pan-y` — lets the browser pick scroll axis from initial swipe direction; horizontal swipe → table scrolls, vertical swipe → page scrolls
- `WebkitOverflowScrolling: touch` — enables momentum scroll on older iOS (belt-and-suspenders)
- **Do NOT use `pan-x` alone** — it blocks vertical page scroll entirely when the finger lands over the element

**Trap:** Playwright `el.scrollLeft = X` succeeds even when touch scroll is broken. To actually test touch, use `devices['iPhone 13']` context + audit `getComputedStyle(el).touchAction` — if it's `"auto"` on a child inside `overflow-hidden`, swipe is broken.
