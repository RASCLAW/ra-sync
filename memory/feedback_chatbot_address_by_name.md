---
name: Chatbot must address customer by name when known
description: When CUSTOMER NAME is in context, bot addresses the person by first name throughout the conversation (sparingly), not just the first message.
type: feedback
related:
  - feedback_chatbot_language.md
  - feedback_conversational_faq.md
  - reference_chatbot_live_path.md
  - project_chatbot_recovery_complete.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
If a customer's first name is provided in the conversation context (fetched from Meta profile API), the chatbot must address the customer by name naturally throughout the conversation -- not just the first message.

**Why:** Session 123, RA flagged chatbot was NOT using the customer's name even though the name was in scope. The earlier rule only mentioned name-use in the FIRST MESSAGE BEHAVIOR block, which Gemini interpreted as "only use name once." The result felt impersonal on longer threads. Using a first name sparingly (1x in first reply, occasionally on reassurance / order confirmation / close) reads warm without being robotic.

**How to apply:**
- Dedicated NAME USAGE block in `cloud-run/conversation_engine.py` SYSTEM_PROMPT (above FIRST MESSAGE BEHAVIOR).
- Cadence guidance: once in the first reply, then occasionally on warmth-beat moments (confirming an order, reassuring a hesitant customer, closing). Not every single reply.
- Never invent a name. If no name is in context, use "Hi there" / "Hey" -- do not guess a name from the phone or username.
- Customer name pulled from Meta profile API in the webhook layer and passed into `generate_reply(customer_name=...)`.
