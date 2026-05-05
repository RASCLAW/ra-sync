---
name: JS Version Tags Must Be Bumped With CSS
description: All script src tags need version bumps alongside CSS — stale JS cache breaks fixes even when CSS is current
type: feedback
related:
  - feedback_version_bump_always.md
originSessionId: d9abb3be-74ff-423d-9e09-bde284084605
---
Bump ALL asset version tags (CSS + every JS `<script src="...?v=...">`) together whenever anything changes.

**Why:** The order page had `order.js?v=v3-010` while CSS and the fix were already at v3-027. The browser served the cached old JS, so the `saveCart()` fix never loaded. Took a second debug round to catch.

**How to apply:** After any CSS or JS change, grep the affected HTML file for `?v=` and bump every one — not just the stylesheet. Especially watch `<script src>` tags in subdirectory pages (e.g. `order/index.html`, `products/item.html`) which tend to lag behind `index.html`.
