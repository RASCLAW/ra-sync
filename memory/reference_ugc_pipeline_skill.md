---
name: UGC Pipeline Skill Rename
description: /ugc-pipeline is the primary UGC content skill. /dubery-v3-pipeline archived.
type: reference
related: [reference_pipeline_skill_chain.md, reference_multi_product_mode.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
Primary UGC content-generation skill is now `/ugc-pipeline`.

**Key facts:**
- Location: `.claude/skills/ugc-pipeline/SKILL.md`
- Archived predecessor: `.claude/skills-archive-v1/dubery-v3-pipeline/` (identical flow; archived session 122)
- Argument hint: `[product] [category] [count]  OR  just [count] for mixed-product batch`
- Chain: invokes `/dubery-fidelity-prompt` for prompt assembly, `/dubery-v3-validator` for pre-spend gate, `generate_vertex.py` for the paid call
- Existing `/dubery-ugc-pipeline` (captions + prompts) is a different, older skill -- DO NOT confuse

**Why:** Session 122 -- RA wanted a shorter invocation path. Renamed/duplicated the v3-pipeline flow into `/ugc-pipeline`, stripped alias language, archived the original to avoid two-source drift.
