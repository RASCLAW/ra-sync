---
name: studio-duberymnl-tunnel
description: studio.duberymnl.com routes to localhost:3002 (Hyperframes Studio default port); use for phone preview of any Hyperframes reel
metadata: 
  node_type: memory
  related: 
    - reference_v3_tunnel.md
    - reference_cc_command_center.md
    - feedback_vercel_preview_over_cf_tunnel.md
    - project_dubery_trailer_v1.md
  type: reference
  originSessionId: 4ac03bd6-5087-46e3-92ef-cc91a144e7a7
---

`studio.duberymnl.com` is permanently wired to `localhost:3002` in `~/.cloudflared/config.yml`. Use it whenever you `npm run dev` in a Hyperframes project — that's the framework's default Studio port.

Why we added it: CF quick tunnels (`cloudflared tunnel --url`) 404 from the edge when named tunnel credentials exist (the chatbot tunnel), which is documented in [[feedback_vercel_preview_over_cf_tunnel.md]]. The fix for Hyperframes specifically was to add a new ingress hostname to the named tunnel rather than fight the quick-tunnel routing.

How to apply:
- Start any Hyperframes project: `cd ~/projects/hyperframes/<project> && npm run dev`
- Studio binds 0.0.0.0:3002 by default — accessible via studio.duberymnl.com on any device.
- Only one Hyperframes Studio can run at a time (port 3002). If you have two reels open, the second will fail to bind.
- Tunnel config: hostname added 2026-05-19, DNS CNAME created via `cloudflared tunnel route dns f2e8c4e2-... studio.duberymnl.com`.
- Named tunnel runs manually (no Windows service) — if it dies, restart with `cloudflared tunnel --config /c/Users/RAS/.cloudflared/config.yml run`.

If port 3002 conflicts, the cleanest fix is to update both `npm run dev -- --port <N>` AND the config.yml ingress to match.
