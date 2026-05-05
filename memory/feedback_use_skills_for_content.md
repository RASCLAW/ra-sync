---
name: Always Use Skills for Content Generation
description: Never bypass the v2 content skills with ad-hoc prompts -- skills encode all the rules, banks, and fidelity guards. Ad-hoc prompts miss critical details.
type: feedback
related: [project_content_skill_iterations.md, project_v2_skills_validated.md, reference_prompt_reviewer_skill.md, feedback_use_reviewer_before_gen.md]
originSessionId: aff172f2-5c57-4baa-b3b0-bc46b873440b
---
Never write ad-hoc image generation prompts. Always use the actual v2 content skills (dubery-ugc-prompt-writer, dubery-brand-callout, dubery-brand-bold, dubery-brand-collection) to generate prompts.

**Why:** Session 114 fidelity scorecard used a hardcoded prompt template in Python instead of the skills. Result: wrong shape descriptions ("rounded classic" for angular Outback), bad posing instructions ("resting closed" caused impossible folding), no inner arm design rules, no finish language. 7/11 failed -- but most failures were prompt bugs, not product fidelity issues. $0.77 wasted. The second batch using actual skills scored 9/11 PASS.

**How to apply:**
- The batch randomizer (`tools/image_gen/batch_randomizer.py`) picks random combos, then the skills write the actual prompts
- If building a new tool that generates prompts, it must route through the skills -- never hardcode prompt templates
- The skills encode R2 fidelity, R3 render_notes, R4 lens reflection, variety banks, framing rules, angle bans -- all things an ad-hoc prompt will miss
