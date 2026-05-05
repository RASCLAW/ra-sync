---
name: v3.duberymnl.com Tunnel
description: v3 landing page is CF tunnel → localhost:8300 (NOT Vercel); all 6 CF subdomain mappings
type: reference
related: [reference_cloudflare_migration.md, project_dubery_v3_landing.md, reference_cloudflare_tunnel_preview.md]
---
**URL:** https://v3.duberymnl.com
**Editor:** https://v3.duberymnl.com?edit
**Local port:** 8300
**Server:** `python -m http.server 8300` from `dubery-landing-v3/`

**IMPORTANT:** v3.duberymnl.com is served by the CF tunnel to localhost:8300, NOT by Vercel. Editing files in `dubery-landing-v3/` reflects immediately — no deploy needed. Running `vercel --prod` does NOT update the live site.

**All 6 CF tunnel subdomain → port mappings** (from `~/.cloudflared/<tunnel-id>.yml`):
| Subdomain | Port | Service |
|-----------|------|---------|
| chatbot.duberymnl.com | :8085 | Flask chatbot |
| cc.duberymnl.com | :8090 | Command Center |
| review.duberymnl.com | :8123 | Image review server |
| tag.duberymnl.com | :8124 | Image tag server |
| v3.duberymnl.com | :8300 | v3 landing page |
| cq.duberymnl.com | :8400 | CQ Assistant |

**Start server:**
`powershell Start-Process python -ArgumentList '-m','http.server','8300' -WorkingDirectory 'C:\Users\RAS\projects\DuberyMNL\dubery-landing-v3' -WindowStyle Hidden`

**PDP gallery editor:** `item-editor.js` — add/remove/reorder photos, Save data.json. All PDPs have a floating `✎ Edit` button. See `reference_v3_pdp_gallery_editor.md`.

**CF tunnel cache:** After updating JS, Ctrl+Shift+R on v3.duberymnl.com to bust browser cache.
