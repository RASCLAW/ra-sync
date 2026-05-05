---
name: AI Prompts Must Block Verbatim Copy
description: Make AI Toolkit parrots client input unless prompt explicitly says DO NOT copy verbatim
type: feedback
related: [feedback_make_router_paths.md, reference_make_zapier.md]
---

When using Make.com AI Toolkit (Ask module) to generate client-facing emails, the AI will copy-paste the client's message as-is unless the prompt explicitly instructs it not to.

**Why:** First test of consulting proposal email opened with the client's exact message as the first sentence -- looked robotic and unprofessional.

**How to apply:** Always include "DO NOT copy the client's message verbatim. Paraphrase their need in your own words." in any AI prompt that references user-submitted text for client-facing output.
