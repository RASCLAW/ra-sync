---
name: No Hardcoded Examples in Skills
description: RA dislikes hardcoded example strings in skill files -- keep skills declarative (rules, structure, templates with placeholders) not prescriptive (specific text strings to copy)
type: feedback
related: project_v3_fidelity_approach.md, feedback_document_testing.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Don't embed specific example strings or literal prompt fragments inside `.claude/skills/` files. The skill file is the contract for HOW to build prompts, not a dumping ground for verbatim examples.

**Why:** Session 120 -- when I added "Safe template: 'Dubery branded box, drawstring pouch with red carabiner, microfiber cloth...'" directly into the v3-pipeline skill, RA rejected the edit. Skills should state the rule (e.g. "keep accessory descriptions minimal") without prescribing exact wording.

**How to apply:** Keep skills at the rule/structure level. Use placeholders like `{frame_direction}`, `{FROM product-specs.json}`. Move specific examples to:
- Memory files (feedback/reference type) if the example is a validated pattern
- Session test logs in `.tmp/` if the example is an in-progress experiment
- Decisions log if the example documents a specific choice
