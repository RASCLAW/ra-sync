---
name: Command Center Online Access
description: cc.duberymnl.com auth URL, port, env vars, desktop shortcut -- how to reach Command Center from phone or external network
type: reference
related:
  - reference_cloudflare_migration.md
  - reference_cloudflare_tunnel_preview.md
  - feedback_cc_dual_instance.md
  - feedback_flask_template_cache.md
originSessionId: 361cc13d-c8d6-4759-8794-444717d74e43
---
Command Center is accessible externally via Cloudflare tunnel at `cc.duberymnl.com`.

**Auth URL (bookmark on phone):**
```
https://cc.duberymnl.com/auth/6458c2c7a50e8f627042f7b22671ccf5
```
Visit once → sets session cookie → redirects to dashboard. After that, `https://cc.duberymnl.com` works directly.

**Local access:** `http://localhost:8090` — no auth required (localhost bypass in middleware).

**Desktop shortcut:** `C:\Users\RAS\Desktop\DuberyMNL Command Center.url` → opens localhost:8090 in browser.

**Port:** `:8090` (fixed via `COMMAND_CENTER_PORT` env var, default 8090).

**Env vars added (session 138):**
- `CC_SECRET_TOKEN` = `6458c2c7a50e8f627042f7b22671ccf5` — secret URL token
- `FLASK_SECRET_KEY` = `f790b0cffd7401078...` — Flask session signing key

**Config:** `~/.cloudflared/config.yml` has `cc.duberymnl.com → http://localhost:8090` ingress.

**How to apply:**
- If the auth URL stops working, check that `CC_SECRET_TOKEN` in `.env` matches the URL token
- Localhost access always works without cookie (for Task Scheduler / local dev)
- If tunnel is restarted, cc.duberymnl.com comes back automatically — same tunnel as chatbot
