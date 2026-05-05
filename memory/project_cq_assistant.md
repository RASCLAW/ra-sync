---
name: CQ Assistant Webapp
description: Flask webapp for VA DOB verification email template filling, runs on port 8400, accessible via cq.duberymnl.com CF tunnel
type: project
related: reference_cloudflare_migration.md, feedback_flask_tunnel_host.md
originSessionId: f84f31d9-fc33-435e-90f9-71c07de15338
---
Flask webapp for RA's personal work use (Informdata CQ emails).

**Files:**
- `c:/Users/RAS/projects/Knowledgebase-informdata/cq_app.py` -- Flask backend, port 8400, Vertex AI Gemini
- `c:/Users/RAS/projects/Knowledgebase-informdata/templates/cq_index.html` -- drag/drop UI
- `c:/Users/RAS/projects/Knowledgebase-informdata/.claude/skills/cq-va.md` -- Claude Code skill (same prompt)

**Access:** `cq.duberymnl.com` (CF tunnel, port 8400)

**Auth:** Vertex AI ADC (same as DuberyMNL chatbot -- `PROJECT=dubery`, `LOCATION=global`)

**Status:** DNS CNAME added, cloudflared config updated. Needs cloudflared process restart to go live.

**Why:** Copilot Basic was making extraction errors on screenshots; Claude's vision model is more accurate.
