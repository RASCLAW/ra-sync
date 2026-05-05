---
name: Always Run Prompt Reviewer Before Image Gen
description: /dubery-prompt-reviewer is mandatory before spending Vertex AI credits. Catches fidelity violations, missing finish, angle defaults, structural issues.
type: feedback
related: [reference_prompt_reviewer_skill.md, feedback_use_skills_for_content.md, project_content_skill_iterations.md]
originSessionId: aff172f2-5c57-4baa-b3b0-bc46b873440b
---
Run `/dubery-prompt-reviewer` on every prompt batch BEFORE running `generate_vertex.py`. No exceptions.

**Why:** Session 114 -- was about to generate 11 images ($0.77) without the quality gate. RA caught it: "did we use a validator for these prompts?" The reviewer found 7 PATCH issues: 4 UGC prompts missing finish in text, 5 prompts on angle -1. Session 107 decision log explicitly states the reviewer is "mandatory before every batch image gen spend."

**How to apply:**
- After writing prompts (via skills), run the reviewer on all files
- Fix all PATCH and FAIL issues before generation
- The workflow is: randomizer → skills → **reviewer** → generate
- Even if you wrote the prompts yourself and think they're clean, run the reviewer -- it catches things humans miss (missing finish field, banned words in scene descriptions, angle defaults)
