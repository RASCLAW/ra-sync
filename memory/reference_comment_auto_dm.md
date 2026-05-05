---
name: Comment Auto-DM (Meta UI)
description: Meta Business Suite comment automation -- nurture DM + 10 keywords + album link. Source of the stale 699 bug (now fixed).
type: reference
related:
  - reference_cloudflare_worker_fallback.md
  - project_chatbot_behavior_aligned.md
  - feedback_worker_ping_rule.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Location:** Meta Business Suite → Inbox → Automations → "Comment to message - PM SENT"

**Current state (session 125):**
- Status: ENABLED
- Trigger: customer comments with any of 10 keywords
- Keywords: hm, how much, magkano, price, order, avail, interested, mine, cod, free shipping
- Action: reply to comment + send private message (DM)

**DM template:**
```
Hey! Saw your comment — thanks for checking us out 😎

What caught your eye? I can help you pick the right pair.

Check out the full lineup: https://www.facebook.com/share/p/1SuARZpPUz/
```

**Funnel:** customer comments on post → keyword match → Meta sends nurture DM → customer replies → message hits Messenger webhook → Gemini handles from there.

**History:** Previously had stale 699/P1,200 pricing template ("Hi! Thanks for inquiring on our post. Dubery Polarized Sunglasses P699 each..."). Discovered session 125 via Christopher Zulueta convo. Root cause: old Meta UI configuration never updated when pricing changed to 599.

**How to apply:** Any pricing/promo changes must be updated in BOTH the chatbot code (knowledge_base.py) AND this Meta UI automation. They are independent systems — code changes don't flow to Meta settings.
