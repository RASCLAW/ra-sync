---
name: Bespoke Content Generation Mode
description: Command Center Bespoke pill -- concept recreation workflow. Paste reference image, agent interprets and recreates with DuberyMNL branding. Skips randomizer, goes straight to fidelity-prompt.
type: project
related: reference_command_center.md, project_command_center_phase2_scoping.md, feedback_form_always_randomizes.md, project_v3_fidelity_approach.md
originSessionId: a4923660-d963-4677-a112-e1a456acf735
---
## What it is
Third mode pill in Content Gen tab alongside UGC and Brand. For concept recreation -- paste any reference image (competitor ad, art, mood photo), agent interprets the visual direction and recreates it with DuberyMNL product + branding.

**Why:** Validated session 133 (2026-04-18). RA pasted a PRADA banner ad and an underwater art piece as concepts. Agent produced high-fidelity recreations with correct product, brand identity, and adapted color palettes. Simple prompt "use the concept of the image attached for duberymnl" worked 3x consecutively.

**How to apply:**
- Bespoke mode skips randomizer entirely (no v3_randomizer or batch_randomizer)
- Goes straight to `/dubery-fidelity-prompt` (v3 product-as-locked-asset schema)
- Scene variables come from interpreting the concept image, not from banks
- Agent color-matches scene accents to the selected product's actual finish
- Validates via `/dubery-v3-validator` then generates via `generate_vertex.py`
- If no concept image attached, agent nudges user to paste one

## Key insight
RA's best results come from art-directing (concept + one sentence) rather than micromanaging prompt details. The agent fills gaps using brand rules + prodref system.
