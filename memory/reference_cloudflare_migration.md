---
name: Cloudflare Tunnel & DNS Setup
description: duberymnl.com DNS on Cloudflare, named tunnel dubery-chatbot for chatbot.duberymnl.com. Phases 1-4 done, waiting on SSL cert.
type: reference
related: [reference_vscode_tunnel.md, project_chatbot_live.md, project_chatbot_knowledge_base.md, reference_chatbot_image_bank.md, reference_cloudflare_tunnel_preview.md, feedback_cloudflare_edge_cache_bust.md]
originSessionId: 1778b02c-2648-418c-a136-be0c24b4cc8f
---
Session 111 (2026-04-12) -- Executed Cloudflare migration Phases 1-4. Runbook at `references/cloudflare-migration-runbook.md`.

**Status:** ALL PHASES DONE (session 117). SSL live, webhook wired, auto-start configured, Worker fallback deployed.

**Cloudflare account:** `sarinasmedia+rasclaw@gmail.com` (free plan)
**Nameservers:** `jerome.ns.cloudflare.com` + `ursula.ns.cloudflare.com`

**Current DNS state (Cloudflare-managed):**
- NS: Cloudflare (jerome + ursula)
- A @ -> `76.76.21.21` (Vercel, proxied)
- CNAME www -> `cname.vercel-dns.com` (Vercel, proxied)
- CNAME chatbot -> `f2e8c4e2-7911-4fdf-bf05-af6dc9d9a6b2.cfargotunnel.com` (named tunnel)
- MX x 3 -> `route{1-3}.mx.cloudflare.net` (Cloudflare Email Routing)
- TXT SPF -> Cloudflare SPF
- TXT DKIM -> Cloudflare DKIM
- Email: `ras@duberymnl.com` -> `sarinasmedia@gmail.com` (verified, active)

**Named tunnel:**
- Name: `dubery-chatbot`
- UUID: `f2e8c4e2-7911-4fdf-bf05-af6dc9d9a6b2`
- Config: `C:\Users\RAS\.cloudflared\config.yml`
- Credentials: `C:\Users\RAS\.cloudflared\f2e8c4e2-7911-4fdf-bf05-af6dc9d9a6b2.json`
- Cert: `C:\Users\RAS\.cloudflared\cert.pem`
- Routes: `chatbot.duberymnl.com` -> `http://localhost:8080`

**How to start manually:**
1. Flask: `cd C:/Users/RAS/projects/DuberyMNL/cloud-run && python messenger_webhook.py`
2. Tunnel: `C:/Users/RAS/bin/cloudflared.exe tunnel run dubery-chatbot`

**Completed:**
- Phase 5: Auto-start via Task Scheduler (DuberyMNL-Chatbot + DuberyMNL-Tunnel) -- session 117
- Phase 6: Meta webhook wired to `https://chatbot.duberymnl.com/webhook` -- session 117
- Phase 7: Cloudflare Worker fallback `dubery-chatbot-fallback` deployed -- session 117
