---
name: Handoff Continuation Rule
description: After handoff flag, bot keeps replying normally. TG ping only on urgent follow-ups (phone+address, ASAP, rush). No silent mode.
type: feedback
related:
  - project_first_closed_order.md
  - feedback_worker_ping_rule.md
  - reference_chatbot_live_path.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Rule:** When `should_handoff: true` fires, the bot continues replying to subsequent messages. No "silent mode" — Gemini handles follow-ups like normal conversation.

**TG notification logic (3 tiers):**
1. **First handoff** → standard 🚨 DuberyMNL HANDOFF ping (existing `notify_tg_handoff`)
2. **Already-handoff + urgent follow-up** → 🔥 URGENT FOLLOW-UP ping (`notify_tg_urgent_followup`)
3. **Already-handoff + non-urgent** → silent (Gemini handles, no ping spam)

**Urgency signals** (`is_urgent_followup()` in messenger_webhook.py):
- Phone number (09xxxxxxxxx) + address keyword = always urgent
- Keywords: urgent, ASAP, rush, ngayon na, today, tonight, tomorrow, deliver na/today/ngayon, kunin na, bili na, order na, paki, pls call/reply

**Why:** RA chose option B over A (silent after handoff) and C (always reply). Reasoning: bot continuing to reply keeps the customer engaged while RA reviews at his own pace. Urgent signals (order info, time pressure) are the only moments where seconds matter — everything else can wait.

**How to apply:** Don't add "bot stops responding after handoff" behavior. The Flask handoff TG ping (`notify_tg_handoff`) fires once; subsequent pings are gated by urgency signals only. Worker-layer pings follow the separate `feedback_worker_ping_rule.md`.
