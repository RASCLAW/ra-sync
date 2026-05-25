---
name: carousel-rasta-red
description: Template A "One Pair, Multiple Looks" carousel for Rasta Red -- 6 slides generated 2026-05-17, pending RA visual review
type: project
related: [feedback_vertex_prompt_json_format.md, project_rasta_red_prodref_issue.md]
---

Template A "One Pair, Multiple Looks" carousel for Rasta Red, generated 2026-05-17.

**Output dir:** `contents/carousel/rasta-red/2026-05-17/`

| Slide | File | Scene | Notes |
|-------|------|-------|-------|
| 01 Hook | slide-01.png | Beach boardwalk, male, white tee | 1.3MB |
| 02 Beach | slide-02.png | White sand beach, female, bikini top | 1.5MB |
| 03 City | slide-03.png | BGC rooftop bar, male, navy polo | 1.7MB |
| 04 Coastal | slide-04.png | Coastal highway overlook, male, denim jacket | 1.5MB |
| 05 Feature | slide-05.png | Holding close-up, rocky coastline | 1.3MB |
| 06 Price | slide-06.png | Copied from existing product shot | 1.5MB |

Slide-07 = reuse slide-01 image with CTA overlay in Meta composer (no separate file needed).

**Copy brief:** `.tmp/carousel-rasta-red-copy-brief.md`
**Prompt sidecars:** `contents/carousel/rasta-red/2026-05-17/slide-0N_prompt.json` (moved there by generator)

**Status:** Pending RA visual review — check lens color (red mirror, not amber/gold) + portrait orientation + product recognizable in each shot.

**Why:** First test carousel for DuberyMNL social feed. Template A = multiple lifestyle looks for one product, designed to drive "which one's you?" engagement.

**How to apply:** If ≥4/5 AI slides approved, add text overlays per copy brief in Meta composer. If lens drift on any slide: add `"not amber, not gold, not yellow"` to that slide's `required_details` and regenerate that slide only (prompt sidecar is in output dir).
