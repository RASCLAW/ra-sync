---
name: bespoke-concept-paste-wins
description: RA's working content-gen pattern is reference-image concept-paste in CC, not text-only randomization. Randomizers can vary scene/lens/headline but cannot invent a strong creative concept.
type: feedback
related:
  - reference_cc_bespoke_pipeline.md
  - feedback_kraft_sidecar_for_fidelity.md
  - project_inspiration_batch_ingestion.md
  - project_iteration_reduction_idea.md
---

Pure text-to-image batches with scene + headline randomization cannot match the quality of the CC bespoke flow (reference image pasted into Direction box, agent extracts concept, structured prompt fired through Vertex).

**Why:** Validated across 4 batches (110+ generations, ~$5 spend) in session 170. Even with premium prompt language (Persol/Ray-Ban/Oakley benchmark, kraft sidecar fidelity, scale caps, no-body-shots, hierarchy rules), text-only outputs read as "lifestyle with overlays" not "ad campaign." The 120+ portfolio-grade ads in `contents/ready/ads/` all came from concept-paste, not randomization. RA's own observation: "creating images from text to image is a bit hard due to creativity... images get too repetitive and lack creativity even if we use a randomizer." The creative idea is the inspiration itself -- randomizers can mix variables, they cannot invent a magazine-cover concept like "DUBERY vertical typography stack framing the model" or "Varilux-style oversized product on gradient backdrop with brand swap."

**How to apply:**
- For RA's content engine, treat the bespoke flow (CC Content Gen tab, settings: bespoke + product + ratio + sku) as the primary path. Text-only batching is for filler at best.
- When RA mentions wanting "more content" or "refill the bank," do NOT propose a randomized text-to-image batch as the first solution. Instead, ask whether he has inspiration screenshots stockpiled.
- If building tooling to scale his content output, the right axis is "process more inspirations per session" (Option B batch ingestion -- see [[project_inspiration_batch_ingestion]]), not "make randomization more creative."
- The bespoke skeleton works because `subject_placement` and `location` in the structured prompt JSON contain concrete design moves extracted from the reference image. See [[reference_cc_bespoke_pipeline]] for the exact output shape.
