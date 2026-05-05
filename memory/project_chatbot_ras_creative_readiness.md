---
name: Chatbot RAS Creative Readiness
description: What the DuberyMNL chatbot codebase is production-ready to offer other clients vs. what needs custom work vs. what needs platform-level build. Use to gate RAS Creative sales conversations.
type: project
related:
  - project_chatbot_employee_discipline.md
  - project_positioning_locked.md
  - project_ras_creative_niches.md
  - reference_cowork_client_framing.md
originSessionId: f2b83343-be79-40bb-b8a6-851c6856aee3
---
**Status as of 2026-04-17 session 127.** The chatbot is production-proven on DuberyMNL (first order closed, 7 guardrails live, full behavior discipline). For RAS Creative clients the engine is reusable but not a platform product yet.

**Session 127 Part 2 (2026-04-17 ~01:00-01:30 local) shipped the owner-admin surface:**
- `/mark-sale/<sender_id>` -- structured CRM capture for manual closes (the gap where Page-Inbox sales were invisible to CRM)
- `/conversations` v2 admin dashboard -- badges for policies/source/order/nurture, one-click Release/Flag/Mark-Sale
- Ad-aware openers Phase 2 -- tailored first reply per `ref` tag from Meta Ads Manager
- System prompt softened (disciplined-employee voice, no reflexive "which model?" pile-on)

**Ready to ship per-client (copy codebase, customize):**
- 7-guardrail discipline stack (turn cap, policy one-shot, complaint, loop guard, phantom asset injector, human takeover, first_name persist)
- 24h time-decay handoff release
- 18h proactive nurture scanner
- Echo-logging (owner's manual replies captured to CRM)
- Ad-referral capture (Phase 1)
- Google Sheets CRM writes (leads, orders, messages, status)
- Telegram handoff pings
- Admin endpoints (`/flag`, `/release`, `/mark-sale`, `/status`, `/conversations` v2 with AJAX buttons, `/chat-test`)
- Ad-aware tailored openers via `chatbot/ad_registry.json` (15 entries, per-variant/per-series/generic)
- Cloudflare Worker fallback pattern when origin is down

**Needs 1-3 days of custom work per client:**
- Knowledge base (products, prices, policies, FAQs, voice) -- ~1 day
- Google Sheet CRM schema setup -- ~half day
- Meta app + webhook config -- ~half day
- Custom policy definitions in `security.POLICY_DEFINITIONS` (each client's non-negotiables) -- ~few hours

**Needs 2-4 weeks of platform work before a "list on Upwork for any small business" scale:**
- Multi-tenancy. Each client today needs own Flask instance. Fine for 1-5 clients, not 50.
- Non-technical admin UI. Config is env vars + Python -- business owner can't self-tune.
- Hosting story. DuberyMNL runs on RA's laptop + Cloudflare Tunnel. Options per client: (a) RA hosts as part of retainer, (b) migrate to Cloud Run/Railway/Render (~1 day), (c) client supplies VPS.

**Pricing model (draft, to validate):**
- One-time setup: PHP 35,000-75,000
- Monthly retainer: PHP 8,000-20,000 (hosting, prompt tuning, CRM reviews, monthly report)
- Ad-attribution add-on: +PHP 5,000 one-time
- Client supplies: Meta Business account, Facebook Page access, brand voice samples, product info

**Pitch line:** *"I install an AI employee for your Messenger. It handles the easy 70% so your team handles the valuable 30%."*

**How to apply:**
- When RA gets a RAS Creative lead interested in chat automation, gate on solar niche first (per `project_ras_creative_niches.md`). If solar/battery-storage: offer this.
- Before quoting a timeline, check which custom-work items apply. Default: 5-7 business days end-to-end for a single solar installer.
- Do NOT pitch the "50 clients on one codebase" version until multi-tenancy + admin UI are built. Push back if a prospect wants that.
- For first 2-3 clients, host on RA's laptop too (pay-for-retainer). Only migrate to cloud hosting once recurring revenue justifies the infra cost.
