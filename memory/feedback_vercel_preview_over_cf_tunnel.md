---
name: Vercel CLI > Cloudflare Quick Tunnel for Phone Preview
description: CF quick tunnels return edge 404 when named tunnel credentials exist; use vercel --yes instead
type: feedback
related: [reference_cloudflare_migration.md, reference_cloudflare_tunnel_preview.md]
originSessionId: 6e1fce92-1052-47da-9764-2e7485ec7e79
---
Use `vercel --yes` (run from inside the project folder) for stable phone preview URLs. Do NOT rely on `cloudflared tunnel --url` quick tunnels for preview sharing.

**Why:** When named Cloudflare tunnel credentials exist in `~/.cloudflared/` (which they do for the chatbot tunnel), quick tunnel URLs get assigned but return HTTP 404 from Cloudflare's edge — the tunnel connects but routing doesn't work. The URL dies or 404s every time.

**How to apply:** When RA says "send to my phone" or "test on mobile", run `cd <project-dir> && vercel --yes` and grab the `Preview:` URL. Takes ~10 seconds, stable until next deploy. Send via `tg-send.py text "<url>"`.
