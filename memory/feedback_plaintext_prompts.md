---
name: Plain Text Prompts for Gemini
description: Use plain text prompts instead of JSON for Gemini image gen -- same quality, no redundancy
type: feedback
related: [feedback_naturalism_prompting.md, feedback_simple_prompts.md, reference_vertex_ai.md]
---

Plain text prompts produce identical quality to structured JSON for Gemini 3.1 Flash image generation.

**Why:** JSON had redundant fields -- structured metadata repeated the same info as the prompt text. Gemini just needs clear text + reference images. The JSON format was designed for kie.ai's API, not for Gemini.

**How to apply:** When generating images via Vertex AI, write 3-5 sentence plain text prompts. Reference images are passed as separate Parts. No need for structured JSON prompt files -- save as .txt if persisting.
