---
name: Skill Creator Skill (Claude Code marketplace)
description: Official Claude Code plugin that A/B benchmarks skills with quantifiable data. Closes the "visual inspection only" gap in RA's skill iteration loop.
type: reference
related:
  - ../../../../projects/EA-brain/references/summaries/chase-top10-skills.md
  - ../../../../projects/EA-brain/references/summaries/nate-herk-superpowers-plugin.md
  - project_content_skill_iterations.md
  - project_v2_skills_validated.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
**What it is:** Official Anthropic marketplace plugin for Claude Code. Runs A/B benchmarks and produces quantifiable before/after metrics when creating or editing skills.

**Why it matters:** RA iterates skills heavily (v2 → v3 fidelity, kraft prodref workflow, ugc-pipeline) but currently relies on visual inspection + RA-confirmed A/B. Skill Creator Skill adds quantitative signal to that loop.

**Install:** `/plugin` command in Claude Code, search "Skill Creator Skill", install from official marketplace.

**How to apply:**
- Use on next MAJOR skill revision (fidelity-prompt, v3-validator, or when building a brand-pipeline-orchestrator from the research backlog).
- Don't install yet if no skill work is imminent — avoid context bloat per Brad's guidance.
- Compare quantitative results with RA's visual-inspection verdict; if they diverge, dig into why.
