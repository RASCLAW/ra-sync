---
name: Flask Template Cache (debug=False)
description: Flask caches templates when debug=False -- server restart required for HTML changes to take effect
type: feedback
related: [reference_cc_command_center.md, feedback_flask_tunnel_host.md]
originSessionId: 233a6356-4e7b-42ea-b6d5-a7aaa1724e0c
---
When Flask runs with `debug=False` (production mode), templates are compiled and cached at startup. Editing a `.html` template file has no effect until the server is restarted.

**Why:** Bit us 2026-04-20 — added mobile nav HTML, bumped CSS version, but served page still showed old markup. Had to `taskkill` + restart.

**How to apply:** After any edit to `command-center/templates/`, always restart the CC server: kill PID on :8090, then `nohup python command-center/app.py`. Static JS/CSS changes take effect immediately (no restart needed).
