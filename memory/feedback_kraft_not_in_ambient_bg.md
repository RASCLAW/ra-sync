---
name: Kraft Studio Shots Don't Belong in Ambient Backgrounds
description: Reserve kraft-bg product shots for product cards / catalog where identity must show clearly. Ambient/floating/drifting backgrounds use UGC wearing + brand editorial only
type: feedback
related:
  - feedback_ugc_framing.md
  - feedback_content_storage_rule.md
  - project_dubery_landing_v2.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
When populating an ambient background (peacock tile-floor, drifting poster wall, parallax field), DO NOT mix in `contents/ready/product/*` kraft-bg studio shots. They read as catalog / e-commerce chrome, not lifestyle — and they fight the ambient-lifestyle feel of UGC wearing shots.

**Why:** Session 129 first pass pulled 226 tiles including ~93 kraft product shots. RA called them out specifically — "use UGC images in the home page less kraft product shots. more UGC wearing and UGC product aesthetic shots." Refiltered to 131 tiles (97 person UGC + 34 brand editorial), immediately looked more premium.

**How to apply:**
- **Ambient backgrounds:** `contents/ready/person/**` (UGC wearing, selfie, holding) + `contents/ready/brand/**` (editorial layouts, NOT BOLD/CALLOUT text-heavy variants). Skip `contents/ready/product/**` entirely.
- **Product cards / catalog / best-sellers rows:** USE kraft shots (`contents/assets/hero/hero-*.png`). These are where product identity needs to be crisp and recognizable.
- **Rule of thumb:** If the image is there to set a mood, use UGC/brand. If it's there to identify a specific SKU for shopping, use kraft.
- **Also skip headband shots** (`CAT2c_ugc_headband*`, etc.) per `feedback_no_headband_outfit.md` — RA rejected that style.
