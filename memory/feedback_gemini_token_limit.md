---
name: Gemini maxOutputTokens 800 Too Low for Chatbot JSON
description: 800 tokens causes truncated structured JSON replies; use 1500+ for chatbot conversation_engine
type: feedback
related: [feedback_chatbot_architecture.md]
originSessionId: 6301781d-2f22-4e59-b1f1-3a4187f641a5
---
Set `maxOutputTokens` to at least `1500` in `conversation_engine.py`'s `generationConfig`.

**Why:** The chatbot asks Gemini for a structured JSON response with `reply_text`, `image_keys`, `should_handoff`, `handoff_reason`, `detected_intent`, `confidence`, and `extracted` fields. At 800 tokens, Gemini hits the limit mid-sentence in `reply_text`, producing truncated JSON that fails `json.loads`. The fallback regex then extracts a partial string — customers received cut-off replies like "Each pair is 599. Shipping starts at 1". Bumped to 1500 in session 136.

**How to apply:** If chatbot replies start getting cut off again, check `maxOutputTokens` first before debugging the JSON parser.
