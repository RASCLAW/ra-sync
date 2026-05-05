---
name: Document Testing Process
description: Always document step-by-step what was done and learned during testing sessions -- don't just generate and show results
type: feedback
related: project_v3_fidelity_approach.md, feedback_prodref_drives_direction.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
When testing (image gen, pipeline, any iterative experiments), document each step as you go:
- What was tested and why
- The exact inputs (prodref, prompt changes, parameters)
- The result (pass/fail, what changed)
- What was learned (the insight, not just the outcome)

**Why:** RA needs to see the reasoning chain to validate the approach and build it into skills/tools later. Showing just the output without explaining the flow means RA can't replicate or map it.

**How to apply:** Before generating, explain the prompt construction steps. After generating, note what worked/failed and why. Keep a running test log in `.tmp/` for the session.
