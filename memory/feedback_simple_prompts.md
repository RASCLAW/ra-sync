---
name: Simple Prompts Win
description: Overloaded prompts cause Gemini 500 errors or forced compositions -- simpler and plainer works better
type: feedback
related: [feedback_naturalism_prompting.md, feedback_plaintext_prompts.md, reference_vertex_ai.md, feedback_stripped_prompt_fields.md]
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Simpler prompts produce better results than overloaded ones with Gemini image gen.

**Why:** Session 88 COMPARISON scenario -- complex prompt with "diagonal split + sunglasses as divider + desaturated treatment + same scene" caused three consecutive 500 errors. Simplified version ("beach split in two halves, left washed out, right vivid, sunglasses in the middle") generated successfully on first try.

**How to apply:**
- Describe WHAT you want plainly, let Gemini figure out HOW
- Don't stack multiple complex concepts in one prompt
- If a prompt 500s twice, simplify it -- the concept is too complex for one generation
- This applies across all image gen, not just brand content
