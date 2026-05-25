---
name: inspiration-batch-ingestion-option-b
description: Backlog plan -- inbox folder + CC "Batch from inbox" button that runs M inspirations through the existing bespoke pipeline in one job. Solves the "I don't have time to copy-paste each inspiration" bottleneck.
type: project
related:
  - feedback_bespoke_concept_paste_wins.md
  - project_iteration_reduction_idea.md
  - reference_cc_bespoke_pipeline.md
  - project_command_center_phase2_scoping.md
---

Extend the CC Content Gen tab to process M concepts × N variants in one batch, not just 1 concept × N variants like the current flow.

**Why:** Session 170 discussion. RA's bottleneck is not the per-concept pipeline (CC already does 1 → 9 variants well, history shows 130+ runs). The bottleneck is when he has 5-10 inspirations stockpiled -- the per-concept paste/direction/settings/generate flow has to run 5-10 times manually. He explicitly chose Option B ("inspiration scraper / mood-board feeder") over Option A (curated concept library).

**Phase 1 (~2-3 hr lift, minimum viable):**
- New folder: `contents/inspirations/inbox/` (RA drops screenshots manually during doom-scroll)
- Filename convention (optional): `concept-name_{sku}.png` → auto-picks prodref; no SKU in name → rotates or uses default
- Optional per-image sidecar `.txt` with overrides (count, ratio, type, sku)
- New tool: `tools/content_gen/batch_bespoke.py`
  - For each file in inbox: spawn Claude agent with same bespoke direction template CC uses today (paste concept + "use duberymnl fonts logo and product")
  - Agent emits structured prompt JSON, runs `generate_vertex.py`
  - Output: `contents/runs/{timestamp}_batch_bespoke/{concept-name}/`
  - Move processed inspiration to `contents/inspirations/processed/`
- New CC tab button: "Batch from inbox" — reuses the existing per-concept pipeline, just loops it

**Phase 2 (~3-5 hr lift, deferred):**
- Direct Pinterest/IG ingestion (saved board URL → auto-download to inbox)
- Pinterest is OAuth-gated; fallback to `gallery-dl` or `pinscrape` libraries
- Lower priority -- Phase 1 unblocks immediately with manual screenshot drops

**How to apply:**
- Pair with [[project_iteration_reduction_idea]] -- auto-rejection is essential when batching 10+ concepts, manual triage of the output flood doesn't scale
- Per-concept pipeline already exists in CC, this just wraps it in a loop -- no new agent logic to write
- Output folder structure (`contents/runs/{timestamp}_bespoke/`) is already the convention -- batch outputs slot in cleanly ([[reference_cc_bespoke_pipeline]])
- This is the right answer for "scale RA's content production" given [[feedback_bespoke_concept_paste_wins]] -- text-only randomization can't fill the gap
