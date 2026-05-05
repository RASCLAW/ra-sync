---
name: Hover Swap Model Shots Preference
description: Model/lifestyle shots preferred for product card hover images, not social graphics
type: feedback
related: [project_dubery_v3_landing.md, project_content_distribution_system.md]
originSessionId: 3e3c3ca2-9c15-482e-b30c-d6038497e5fb
---
Use model or lifestyle shots for the `.bs-img.hover` swap on product cards — not social graphics or UGC posts.

**Why:** RA explicitly flagged this after seeing the first batch of hover images (which were social-pool images). Model shots show the product being worn, which is the right context for a catalog hover state.

**How to apply:** When sourcing or generating hover images for `data.json`, pull from `contents/ready/person/` model shots or lifestyle shots, not `assets/social/social-0N.png` pool images.
