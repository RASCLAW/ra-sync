---
name: Oracle Cloud VM as long-term chatbot home
description: Oracle Always Free tier (24 GB RAM, 4 ARM vCPU) is the planned migration target for DuberyMNL chatbot + Rasclaw + Belle bots. Forever free, one VM hosts everything.
type: project
related: [project_chatbot_live.md, reference_cloud_run_cpu_throttle.md, project_rasclaw_telegram.md, project_belle_telegram.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
Oracle Cloud Infrastructure Always Free tier is the long-term home for all of RA's always-on services.

**What's free, forever (not a trial):**
- 1 ARM VM with 4 OCPU + 24 GB RAM (or 2× AMD micro VMs with 1 GB each)
- 200 GB block storage
- 10 TB/mo outbound bandwidth
- Public IPv4 address
- No expiration

**Why:** GCP free trial ($300 credits) expires 2026-07-06. DuberyMNL chatbot on Cloud Run burns ~$7–8/mo after Option A (session 99), which eats ~$50 of credits. Before trial ends, need a permanent home. Oracle VM also solves Rasclaw + Belle bot hosting (both on backlog needing always-on infrastructure).

**How to apply:**
- Single VM hosts multi-service stack: chatbot + Rasclaw + Belle + future projects
- Use Docker Compose for deployment + service isolation
- Caddy or nginx for TLS + reverse proxy (Meta webhooks need HTTPS)
- Cloudflare optional for extra DDoS/caching layer
- Monitor via uptimerobot.com (free) + optional Grafana Cloud free tier
- DNS via Namecheap (already owns duberymnl.com)

**Migration order (when ready):**
1. Provision Oracle VM (first-time setup ~2–4 hrs)
2. Deploy chatbot in parallel (different webhook URL for testing)
3. Smoke test with TEST_BATTERY sender IDs
4. Swap Meta webhook subscription URL to Oracle
5. Monitor 24h, then decommission Cloud Run service
6. Layer Rasclaw + Belle onto same VM

**Portfolio angle:** "deployed and operate a multi-service stack on Oracle Cloud Infrastructure" is a stronger interview talking point than "clicked deploy on Cloud Run". Relevant for RA's AI/automation career pivot.

**Status:** RA committed to starting Oracle setup today (2026-04-10) to avoid rushing in June. Chatbot currently fine on Option A Cloud Run config — no urgency to migrate the code path itself.
