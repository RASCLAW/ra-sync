---
name: Filipino customer shorthand for chatbot
description: Short message patterns Filipino customers use that the chatbot must interpret correctly — especially "hm" = "how much", not a confused noise.
type: reference
related: [feedback_chatbot_language.md, project_chatbot_live.md, project_messenger_strategy.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
Filipino customers on Messenger use very short shorthand that non-PH bots misinterpret. The DuberyMNL chatbot must recognize these patterns or it'll either silence itself (fallback bug) or send the wrong response (generic greeting instead of price).

**Pricing shorthand:**
- `hm`, `Hm`, `hm?`, `Hm po`, `hmp` — "how much" (the most common Filipino pricing shorthand). NOT a confused noise, NOT a "?" signal. RA confirmed this from real customer traffic.
- `magkano`, `mgkno`, `mgkn` — "how much" in Tagalog, sometimes typed without vowels
- `pric?`, `pric` — "price?"

**Greeting shorthand:**
- `hi`, `hello`, `hey`, `yo`, `kmsta`, `kumusta`, `musta` — all greetings

**Acknowledgment shorthand:**
- `ok`, `sige`, `noted`, `k` — acknowledgment, wait for next turn

**Ambiguous:**
- `?`, `...`, single emoji — not enough to infer intent, ask a clarifying question

**Why:** Session 99 — during the refactor verification, "Hm" was returning a generic greeting ("Hey! What can I help you with?") because I assumed it was a confused noise. RA corrected: "hm is actually short for how much." This is the kind of PH customer-behavior knowledge that doesn't exist in Gemini's training data — it has to be explicitly in the system prompt.

**How to apply:**
- The system prompt in `cloud-run/conversation_engine.py` has a `SHORT / UNCLEAR MESSAGES` section that lists these patterns with the expected response for each. Update it when new patterns surface from customer data.
- Test new shorthand in `/chat-test` before shipping. Each new pattern needs at least one smoke test case.
- This list will grow. Don't assume it's complete. When the bot responds weirdly to a short customer message, the first question is: "is this a shorthand I haven't taught it yet?"
