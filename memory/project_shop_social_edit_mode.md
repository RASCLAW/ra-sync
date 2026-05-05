---
name: Shop Social Edit Mode
description: ?edit mode on shop-social page -- remove tiles, edit copy/tags, download updated data.json
type: project
related: [reference_v3_pdp_gallery_editor.md, feedback_nested_button_html.md]
---

Shop-social page (`dubery-landing-v3/shop-social/`) has a `?edit` mode for curating the UGC wall.

**URL:** `v3.duberymnl.com/shop-social/?edit`

**Features:**
- X button on each tile removes it from the in-memory data array and re-renders
- Clicking a tile opens a right-drawer edit panel: author, location, caption, product chips (toggle which slugs are tagged)
- "Download data.json" button in the bottom edit bar exports the current state
- "Exit Edit" link returns to normal view

**Workflow:**
1. Go to `?edit`
2. Remove unwanted tiles via X, edit copy/tags by clicking tiles
3. Hit Download → drop file into `dubery-landing-v3/shop-social/data.json`

**Why:** Static site served by `python -m http.server 8300` — no backend for file writes. Download pattern matches the PDP gallery editor approach.

**How to apply:** When RA says "I updated the feed" or drops a new data.json from Downloads, just copy it to the shop-social directory.
