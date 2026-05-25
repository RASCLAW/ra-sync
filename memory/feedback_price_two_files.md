---
name: feedback_price_two_files
description: Price changes must be updated in both knowledge_base.py AND conversation_engine.py -- they are independent
metadata: 
  node_type: memory
  type: feedback
  related: 
    - project_chatbot_knowledge_base.md
    - feedback_chatbot_architecture.md
  originSessionId: 20a9afa9-b6a1-4124-b19a-0fa178f77a98
---

Always update prices in **both** files when the price changes:
- `chatbot/knowledge_base.py` -- `PRICING` dict, `get_pricing_text()`
- `chatbot/conversation_engine.py` -- hardcoded in 4 places: discount rule, peso example, Phase 2 opener template, bundle rule, and the few-shot pricing example

**Why:** Discovered 2026-05-15 stress test -- bot was quoting 599 after price dropped to 499. `knowledge_base.py` had 499 but `conversation_engine.py` was never updated. Gemini pulls from the system prompt which includes conversation_engine.py strings, not knowledge_base.py alone.

**How to apply:** Any time RA changes pricing, grep both files for the old price number and update all instances. Search: `grep -n "599\|499" chatbot/conversation_engine.py chatbot/knowledge_base.py`
