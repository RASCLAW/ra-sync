---
name: Fidelity Prefix for Gemini
description: Updated mandatory prompt prefix that tells Gemini to preserve product identity -- prevents hallucination on person-wearing shots
type: feedback
related: project_v3_fidelity_approach.md, feedback_naturalism_prompting.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Use this prefix for all image generation prompts:

"Generate an image based on the following JSON parameters and the attached reference image - ensure that product attached keeps its identity and design do not hallucinate:"

**Why:** Session 119 -- the old prefix ("Generate an image based on the following JSON parameters and the attached reference image:") caused product identity loss on UGC_PERSON_WEARING shots. Frame shape went generic wayfarer, lost angular flat-top. The stronger prefix with explicit identity preservation instruction improved fidelity noticeably.

**How to apply:** Replace the old prefix in dubery-fidelity-prompt and dubery-v3-pipeline skills. This is now the locked mandatory prefix.
