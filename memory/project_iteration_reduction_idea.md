---
name: iteration-reduction-pre-post-flight
description: Backlog plan -- pre-flight check (kraft sidecar + product-specs validation) + post-flight check (vision-model validation of output vs. spec) could cut bespoke iteration cycles ~40-50%.
type: project
related:
  - project_inspiration_batch_ingestion.md
  - feedback_kraft_sidecar_for_fidelity.md
  - feedback_fidelity_anchor.md
---

Two-layer rejection system to cut iteration in the CC bespoke flow:

**Pre-flight (small lift, ~1 hr):** Extend the prompt builder to read kraft sidecar (`frame_direction`, `visible_details`) + product-specs (lens type: mirrored vs. non-mirrored) BEFORE generation. Refuse to fire prompts that violate these constraints. Catches:
- Compositions asking for angles the prodref doesn't show (e.g., "underside of upper rim" when prodref is 3/4 view)
- Lens tint requests that don't match the SKU spec (e.g., asking for mirrored on glossy-black which is non-mirrored)
- Compositions that fight the prodref's native `frame_direction` (e.g., counter-clockwise tilt on a right-facing reference)

**Post-flight (medium lift, ~2-3 hr):** After each Vertex generation, send the output image + the original spec to a small Gemini Flash vision check: "Does the lens tint match? Is the frame geometry undistorted? Are required_details visible? Is text spelled correctly?" Auto-refire failed outputs with flagged fixes. User only sees outputs that passed.

**Why:** Session 170 watched RA iterate live in CC -- 1 concept → 9 generations across 3 rounds to get 5-6 keepers. Failure modes were specific and predictable: lens fidelity drift, distorted frames, impossible angles. Most iterations are catching mechanical defects, not subjective taste mismatches. Pre-flight + post-flight would filter mechanical fails before RA sees them, leaving only subjective rounds. Estimated 40-50% iteration reduction; faster review cycles; cleaner output stream.

**How to apply:**
- Pre-flight is the cheaper win, build it first
- Auto-rejection becomes essential if/when Option B batch ingestion ships (see [[project_inspiration_batch_ingestion]]) -- manual triage of 10+ inspirations × 9 variants breaks down without a quality gate
- Tied to bespoke-flow because that's RA's primary content engine ([[feedback_bespoke_concept_paste_wins]])
- Vision-check model choice: use Gemini Flash (cheap, fast) not Opus -- this is a binary spec-match check, not subjective design review
- Fidelity anchor + kraft sidecar are inputs to both layers ([[feedback_kraft_sidecar_for_fidelity]], [[feedback_fidelity_anchor]])
