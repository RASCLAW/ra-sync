---
name: Image Generation Auto-Versioning
description: generate_vertex.py auto-bumps to -v2, -v3, etc when output file exists -- no overwrites
type: feedback
related: feedback_use_skills_for_content.md, project_v3_fidelity_approach.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
`tools/image_gen/generate_vertex.py` auto-versions output paths. If `01-hero.png` exists, the next call saves to `01-hero-v2.png`, then `-v3`, etc. The prompt JSON sidecar follows the same versioned name (e.g., `01-hero-v2_prompt.json`).

**Why:** Session 121 burned a paid Vertex call to recover a v2 kraft prodref that got overwritten by v3/v4/v5 iterations. Each regen wrote to the same filename, destroying earlier outputs. RA wanted no more lost work.

**How to apply:**
- Don't manually rename outputs to add `-v2` -- the tool does it
- The "final accepted" version stays at the unversioned name when RA picks it (rename `-v3` to base manually, or generate fresh after deleting failed attempts)
- After RA picks a version, optionally clean up the rejected `-vN.png` and `-vN_prompt.json` files
- This applies to ALL Vertex generations (kraft prodrefs, UGC, brand content)
