---
name: Chatbot prices must use plain numbers (no peso prefix)
description: Chatbot replies must write prices as plain numbers (599, 1200, 100). No "P", "PHP", or "₱" prefix.
type: feedback
related:
  - feedback_chatbot_language.md
  - feedback_conversational_faq.md
  - reference_chatbot_live_path.md
  - reference_dubery_crm_sheet.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
Chatbot replies must render prices as plain numbers: `599`, `1200`, `100`. Never prefix with `P`, `PHP`, `₱`, or currency symbols. Customers already know the currency.

**Why:** Session 123, RA flagged chatbot was prefixing every price with "P" (e.g. "P599", "P1,099"). It reads noisy and adds no information in a PH-only Messenger context. Plain numbers match how customers themselves write prices ("okay for 599?") and feel more conversational.

**How to apply:**
- Rule lives in `cloud-run/conversation_engine.py` SECURITY RULES block.
- `get_pricing_text()` in `cloud-run/knowledge_base.py` outputs plain numbers; FAQ answers that mention shipping also use plain numbers.
- When writing future examples, ad copy, or landing page price tags, only the website storefront UI keeps currency symbols (visual price tags are a separate convention). Chatbot and ad COPY stay plain.
- This works alongside the English-first language rule (feedback_chatbot_language.md) -- both enforce a conversational register.
