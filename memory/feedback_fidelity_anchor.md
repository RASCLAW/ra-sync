---
name: Fidelity Anchor Verbatim
description: Exact no-drift anchor line that must be the last item in required_details on every v3 prompt -- sourced from rasta-brown bespoke prompt, confirmed to fix fidelity drift
type: feedback
related:
  - project_v3_fidelity_approach.md
  - feedback_prodref_drives_direction.md
originSessionId: eed28772-6b1b-4570-aa8e-6c411a942314
---
Always append this exact verbatim as the **final item** in `required_details` in every v3 prompt JSON:

> "Match product proportions, frame shape, temple pattern, emblem placement, lens color, and branding 100% against the attached reference photo -- no drift"

**Why:** Discovered in the rasta-brown bespoke prompt in `.tmp/`. It's a plain-language anchor that tells Gemini "stop drifting, just match the photo." Confirmed it pushed fidelity from generic frames (wrong shape/color) to correct product in the Bandits Blue run (-006 vs -007 comparison). The specificity of "temple pattern, emblem placement" matters -- generic phrases like "all visible details" don't have the same effect.

**How to apply:** This is now a permanent rule in `.claude/skills/dubery-fidelity-prompt/SKILL.md`. Every prompt built through that skill automatically includes it. Do NOT change the wording. If reviewing a prompt JSON that's missing this line, add it before generation.
