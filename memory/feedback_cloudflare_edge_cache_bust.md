---
name: Cloudflare Edge Caches Are Per-Geography
description: CF edge caches static assets per geographic node — dev laptop and remote user may see different cache state. Always cache-bust per-file during iteration
type: feedback
related:
  - reference_cloudflare_migration.md
  - reference_cloudflare_tunnel_preview.md
  - feedback_version_bump_always.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
When iterating on a static site served through a Cloudflare tunnel, a local `curl` showing fresh CSS DOES NOT guarantee RA (or any remote user) sees fresh CSS. Each CF edge node caches independently. RA's work network may hit an edge that still serves yesterday's dark-mode stylesheet even after the tunnel origin has the new one.

**Why:** Session 129 debugging session — I iterated on styles.css on my laptop, verified local + tunnel both served the new file, but RA kept seeing the old dark version from his office network. Diagnosed by having him view `styles.css` directly in browser — he saw the stale "Dark editorial" header. Cache-bust `?v=<tag>` on every asset reference fixed it immediately.

**How to apply:**
- When making CSS/JS/image changes, bump a version tag on EVERY reference: `styles.css?v=flow1`, `script.js?v=flow1`, `<img src="hero.png?v=flow1">`, etc.
- Don't trust `curl https://...` from your own network. Ask RA to hard-refresh AND to paste back what he sees if the iteration doesn't land.
- Use a single short version tag per change set (e.g. `fit1` → `fit2` → `flow1`) to make it easy to burn-bust everything at once with one find-and-replace.
- The peacock tile-mix image URLs needed individual cache-busts (`tile-mix/tile-001.jpg?v=scrub1`) because Cloudflare cached them too.
