---
name: Flask Must Bind 0.0.0.0 for Cloudflare Tunnel
description: Flask bound to 127.0.0.1 blocks Cloudflare tunnel traffic -- must use 0.0.0.0 for any externally-exposed service
type: feedback
related:
  - reference_cc_command_center.md
  - reference_cloudflare_migration.md
originSessionId: 361cc13d-c8d6-4759-8794-444717d74e43
---
Flask defaults to `host="127.0.0.1"` which only accepts loopback connections. Cloudflare tunnel forwards traffic from the tunnel daemon to localhost, but it originates from a different process context — so `127.0.0.1` rejects it and the tunnel returns 502.

**Fix:** Always use `host="0.0.0.0"` in `app.run()` for any service that needs to be reachable via Cloudflare tunnel.

**Why:** Discovered session 138 when wiring `cc.duberymnl.com` — chatbot was already on `0.0.0.0` (no issue), but Command Center was on `127.0.0.1` and would have 502'd from the tunnel.

**How to apply:**
- Any new Flask service exposed via Cloudflare tunnel: `app.run(host="0.0.0.0", port=PORT)`
- Add IP-based auth gating (session cookie / token) before changing host — don't expose raw services without auth
- Localhost-only services stay on `127.0.0.1` intentionally
