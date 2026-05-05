---
name: Chatbot Language Style
description: Chatbot must be English-first even when customer uses Tagalog -- soft rules produced leakage, explicit bans needed
type: feedback
related: [feedback_chatbot_architecture.md, project_messenger_strategy.md, project_chatbot_knowledge_base.md, reference_ph_customer_shorthand.md, feedback_chatbot_no_model_codes.md, feedback_chatbot_no_peso_prefix.md, feedback_chatbot_address_by_name.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
Chatbot must reply in English even when the customer writes in Tagalog or Taglish. The only Filipino sprinkles allowed: "sige", "noted", "ayos", "po" (one max per reply, only at start or end). NEVER a full Tagalog sentence.

**Why:** Session 98 -- initial rule said "95% English with casual Filipino sprinkles" but the bot still replied "Ayos naman ako. Ano pong hanap niyo?" when customer said "Kmsta". The soft rule wasn't enough. Gemini mirrors the customer's language unless the prompt is very explicit. Adding concrete examples + banned phrases fixed it.

**How to apply:** For any language-strict chatbot:
1. Explicitly ban full sentences in the other language, not just soft-discourage
2. Give concrete examples: "Kmsta" -> "Hey! What can I help you with?" (NOT "Ayos naman ako")
3. List specific banned phrases ("Ayos naman ako", "Ano pong hanap niyo", "Kumusta", "Salamat po")
4. Limit allowed sprinkles to 1 per reply

Related but separate: RA was OK with the bot saying "we" -- the I-only rule was too strict and got reverted. "We is fine."
