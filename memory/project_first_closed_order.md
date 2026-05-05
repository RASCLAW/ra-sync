---
name: First Closed Order Milestone
description: 2026-04-15 Kingpin Dela Cruz closed Outback Blue through Gemini chatbot -- validated hybrid AI+HITL architecture, ended Cloud Run migration debate
type: project
related:
  - project_chatbot_recovery_complete.md
  - reference_chatbot_live_path.md
  - reference_cloudflare_worker_fallback.md
originSessionId: ec69ca92-4b44-470f-a2c1-88e55b8f2d58
---
**Milestone:** First real customer order closed end-to-end through the DuberyMNL chatbot.

**Date:** 2026-04-15 (session 124)
**Customer:** Kingpin Dela Cruz (profile name in Arabic script: ديلا كروز مسيحي)
**Order:** 1x Outback Blue, same-day delivery 2pm, Palar Village Scorpion St Taguig, 599 + shipping, COD
**Conversation:** 39 messages over 70 minutes

## Phase 1 -- Gemini handled (16:51-17:15 UTC)
- Customer said "how" (= "how much")
- Uploaded 2 product screenshots -- one had stale 699 pricing
- **Gemini recognized the stale 699 price IN THE IMAGE and corrected it to 599 with explanation** — image-aware + catalog-aware in one reply
- Identified products from the screenshots (Bandits Glossy Black, Outback Black)
- Customer asked "How to order" -> Gemini sent 7-field form
- Customer filled ALL 7 fields (name, address, landmarks, phone, model, preference, time)
- Gemini parsed correctly, confirmed total + shipping range, handed off with "The owner will message you shortly to confirm..."

## Phase 2 -- RA manual (17:39-18:01 UTC)
- 24min gap, customer went quiet after Gemini's handoff
- RA noticed in Messenger (no TG ping -- FAQ+TG upgrade not yet deployed)
- Customer changed mind: Bandits Glossy Black -> Outback Blue
- RA negotiated delivery time: "Around lunch?" -> "After po" -> "2pm?" -> "Noted. Thanks!"

## Why it matters
1. **Validates the hybrid architecture** over the Cloud Run path we abandoned mid-deploy.
2. **Proves Gemini's image+context capability in production** -- no keyword FAQ replaces "I see 699 in your screenshot, that's old, actually 599 now."
3. **Case study for RAS Creative SOLUTIONS** -- solar installers want this exact pattern (AI covers routine, owner covers judgment).
4. **Reinforces TG notification urgency** -- RA stumbled on this order manually; with TG ping he would've jumped in sooner during the 24min quiet gap.

## How to apply
- **Never dilute the Gemini Phase 1 experience** -- don't strip features to fit into the Worker. Worker FAQ templates are fallback, not replacement.
- **Order-form format locked at 7 fields** (Gemini's version). Worker's how-to-order template should match exactly so customers see consistent prompt regardless of origin state.
- **Portfolio evidence**: redact customer PII, annotate Phase 1 vs Phase 2 handoff, use as hero example when pitching hybrid AI automation to SMBs.
- **When evaluating migration/refactor tradeoffs**, reference this thread -- the laptop stack closed a real order while we were 3 hours into a Cloud Run deploy that never worked. Pragmatism > architectural purity.
