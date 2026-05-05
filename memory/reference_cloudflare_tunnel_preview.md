---
name: Cloudflare Named Tunnel as Preview Host
description: review.duberymnl.com + tag.duberymnl.com are already mapped in the named tunnel — use them to preview any sandbox directory without Vercel auth friction
type: reference
related:
  - reference_cloudflare_migration.md
  - reference_review_dashboard.md
  - project_dubery_landing_v2.md
  - reference_cc_command_center.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
The named Cloudflare tunnel has four hostnames wired in `C:\Users\RAS\.cloudflared\config.yml`:

- `chatbot.duberymnl.com → http://localhost:8085` (production chatbot)
- `cc.duberymnl.com → http://localhost:8090` (Command Center — auth-gated, session 138)
- `review.duberymnl.com → http://localhost:8123` (image review dashboard normally, reusable as sandbox preview)
- `tag.duberymnl.com → http://localhost:8124` (tag dashboard normally, reusable)

**How to use as preview host:**

1. `cd` into sandbox dir (e.g. `dubery-landing-v2/`)
2. `python -m http.server 8123` (or 8124 for tag subdomain)
3. Site is immediately live at `https://review.duberymnl.com/` for anyone on the internet

**Why:** Vercel preview URLs are auth-gated (401 on anon access). Saving a dev cycle per iteration matters when RA can't access local/Vercel from his work network. The tunnel is already running as scheduled task `DuberyMNL-Tunnel`.

**How to apply:**
- Don't build a new Vercel preview project just to show RA a dev build. Use the tunnel.
- If the port is already taken by the normal service (review dashboard is normally on 8123), kill the normal service first or use 8124.
- If cloudflared process gets killed (e.g. during a tunnel quick-tunnel attempt), restart via `schtasks /run /tn DuberyMNL-Tunnel` to bring all three hostnames back up.
- Cache-bust per-file with `?v=<tag>` query strings when iterating — CF edge caches per geography, RA's work network hits a different edge than the dev laptop. See `feedback_cloudflare_edge_cache_bust.md`.
