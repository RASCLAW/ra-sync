---
name: FAQ Must Be Conversational
description: Chatbot FAQ answers should sound like a friendly Filipino shop assistant, not a spec sheet
type: feedback
related:
  - project_chatbot_recovery_complete.md
  - feedback_chatbot_language.md
  - project_chatbot_knowledge_base.md
  - feedback_chatbot_no_model_codes.md
  - feedback_chatbot_no_peso_prefix.md
  - feedback_chatbot_address_by_name.md
originSessionId: b1211702-4f57-4a72-a5b5-98cb55c11c8f
---
FAQ answers in the chatbot knowledge base must be written as natural conversation, not terse data points.

**Why:** RA tested the chatbot and flagged pricing/shipping replies as "templated" -- Gemini was parroting spec-sheet FAQ entries verbatim. Conversational source material produces conversational replies.

**How to apply:** When writing or updating FAQ entries in `cloud-run/knowledge_base.py`, write them as if a real Filipino shop assistant is explaining to a customer. Use full sentences, casual tone, "po" where natural. Example:

BAD: "COD (Metro Manila only), GCash, or bank transfer/InstaPay. For provincial orders, prepaid only."

GOOD: "We accept GCash, bank transfer, or InstaPay. If you're in Metro Manila, COD is also available -- just pay the rider when it arrives. For orders outside Metro Manila, we'll need payment first before we ship."
