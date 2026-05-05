---
name: Chatbot must never mention internal model codes
description: Chatbot replies must refer to products by name (Bandits, Outback, Rasta) and color only. Never mention SKU codes D518, D918, D008.
type: feedback
related:
  - feedback_chatbot_language.md
  - feedback_conversational_faq.md
  - reference_chatbot_live_path.md
  - project_chatbot_recovery_complete.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
Chatbot replies must NEVER include internal SKU codes (D518 for Bandits, D918 for Outback, D008 for Rasta, or any similar code). Customers should only ever see product names + variant colors.

**Why:** Session 123, RA flagged chatbot was leaking internal model codes in replies. Model codes are supplier/SKU references, not consumer-facing. Mentioning them makes the bot sound like a reseller pulling from a catalog instead of a native Dubery assistant, and invites customers to search for those codes (which surfaces other sellers).

**How to apply:**
- Rule lives in `cloud-run/conversation_engine.py` SECURITY RULES block.
- `get_catalog_text()` in `cloud-run/knowledge_base.py` deliberately omits the `code` field from its system-prompt output (the dict still has `code` for internal reference only).
- When editing catalog rendering anywhere else (other chatbots, landing page, ads), apply the same rule: code is a private key, name + color is the public identifier.
- If a customer mentions a model code (e.g. they saw it on a supplier site), the bot still does not confirm or repeat it -- reply using the Dubery name.
