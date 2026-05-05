---
name: CC Dual Instance Conflict
description: Two CC processes on port 8090 cause Cloudflare 524 timeouts -- always use cc.duberymnl.com on both devices
type: feedback
related: [reference_cc_command_center.md, feedback_flask_tunnel_host.md]
originSessionId: 233a6356-4e7b-42ea-b6d5-a7aaa1724e0c
---
Only one Flask process should own port 8090 at a time. If you open CC in a PC browser while a background process is already running, the browser spawns a second instance that conflicts.

**Why:** When the tunnel hits the wrong process or a mid-restart gap, Cloudflare returns 524 (origin timeout). Happened 2026-04-20 after server restart.

**How to apply:** Always access CC via `cc.duberymnl.com` on both PC and phone — never open `localhost:8090` in a second browser window while the background process is running. If you need to restart, kill the background PID first (`netstat -ano | grep :8090`).
