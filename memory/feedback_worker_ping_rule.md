---
name: Worker TG Ping Rule
description: Cloudflare Worker sends TG pings ONLY for order_intent. FAQ matches and polite holds stay silent. Noise reduction by design.
type: feedback
related:
  - reference_cloudflare_worker_fallback.md
  - project_chatbot_recovery_complete.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Rule:** The `dubery-chatbot-fallback` Cloudflare Worker sends Telegram pings ONLY when the customer message classifies as `order_intent` (phone number + address keyword). Every other fallback situation — FAQ match, dedup hit, follow-up after canned reply, generic polite hold — stays silent.

**Why:** Most offline-window customers are price-askers who ghost after the canned reply. Pinging on every "how much" generates noise without enabling action. Order intent is the one moment where the seconds matter (customer ready to buy, needs human confirmation). Keeping TG tied to moments-that-need-hands preserves signal.

**How to apply:**
- Never re-add `🔔 customer waiting` or `🔁 follow-up` pings to the Worker fallback path.
- The Flask bot's `notify_tg_handoff()` (Gemini-flagged handoffs) is a separate signal — stays as-is.
- If a new FAQ intent is added later (legit-check, stock-check, reviews), default to no ping unless it contains action-requiring state.
- This rule is Worker-layer only. Dashboard, ads, or pipeline alerts follow their own ping rules.

**Origin:** Decided 2026-04-16 session 124 continuation after the initial 3-flavor ping rollout (🔔/🔁/🚨). RA observed that FAQ-answered customers often stay silent after the canned reply, so 🔔 pings were noise rather than signal. Strip deploy: Worker v2 `a29b0757`.
