---
name: DuberyMNL Project State
description: DuberyMNL sunglasses business — pipeline, tools, ads, current status
type: project
---

**What:** DuberyMNL sells Dubery polarized sunglasses via Facebook. Primary channel is FB page + Messenger.

**Why it matters:** This is RA's live portfolio piece — proof of work for landing a remote AI/automation job.

**Stack:** Claude Code (primary), kie.ai / Nano Banana 2 (image gen), Meta Ads API, Google Sheets (pipeline tracker), Google Drive MCP, Python tools.

**Pipeline (WAT framework):**
- WF1: Caption generation + approval — DONE
- WF2: AI image generation (kie.ai) — DONE
- WF3a: Auto-posting to Facebook — NOT YET (Page Access Token exists, schedule_post.py built)
- WF3b: Meta Ads staging — stage_ad.py exists, needs audit
- WF4: Chatbot — ON HOLD (pivoted to portfolio chatbot)

**Ads:** Traffic v2 campaign (P100/day, 18-45 targeting, no Advantage+). Dayparting via GitHub Actions (schedulers repo). PERSON anchor beats PRODUCT (4.59% vs 3.56% CTR).

**Content pipeline:** 36 IMAGE_APPROVED sitting idle. CEO agent proposed 7 posts/week (4 UGC + 3 ad reposts). NB2 via Gemini = 15-30 free images/day at 1K res (potential kie.ai replacement for drafts).

**Google Drive:**
- DuberyMNL Pipeline sheet (ID: 1LVshSQP5Ob9RNqt35PoSjbUuAiu9dneyHHhUiUZKYrg)
- DuberyMNL Content sheet (captions + review data)
- DuberyMNL Orders sheet

**Products:** Dubery polarized sunglasses — 3 lines:
- Bandits (Matte Black, Glossy Black, Green, Blue, Tortoise)
- Outback (Black, Blue, Red, Green)
- Rasta (Red, Brown)
- Classic series: ARCHIVED (out of stock)
- Pricing: P699 single, P1,200 bundle (2 pairs). Free delivery. COD available.

**Landing page:** duberymnl.com (Namecheap domain → Vercel). Dynamic per-ad via ?id= param. Order form → Google Sheet. Dark theme, mobile-first.

**Domain:** duberymnl.com. Email forwarding: ras@duberymnl.com → sarinasmedia@gmail.com

**Kiko character:** Recurring comic strip protagonist. 24yo Filipino, chin-length dreadlocks, Dubery Rasta Red, calm archetype. Barkada: Dodong + Ces. Character bible + reference image exist.

**Key architectural decisions:**
- n8n workflows scrapped early — went full agentic with Claude Code as orchestrator (session 3)
- Google Sheets is source of truth for manual edits, pipeline.json is local cache (session 47)
- JSON structured prompts > natural language for kie.ai (better product fidelity, session 36)
- Overlay badge shapes are per-concept, not hardcoded — derive from scene energy (session 8)
- 70% PRODUCT / 30% PERSON visual anchor bias (session 34)
- Prompt writer: never describe frame appearance from product name — reference image is only authority (session 38)

**How to apply:** Check pipeline status before suggesting content work. Respect the WAT layers — workflows define what, agent decides how, tools execute.
