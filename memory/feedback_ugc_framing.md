---
name: UGC Framing Rule -- Product Must Be Recognizable
description: UGC shots must keep the product prominently framed. No whole-body wides where sunglasses become unrecognizable. Tight/medium crops only for person-anchor.
type: feedback
related: [feedback_ugc_quality.md, feedback_creative_range.md, feedback_ra_aesthetic_preference.md, project_content_skill_iterations.md, project_v2_skills_validated.md]
originSessionId: e2edcd0d-2636-4f51-925e-1ad296434a55
---
UGC images must frame the sunglasses prominently. No whole-body wide environmental shots where the product is reduced to a dot in the frame.

**Why:** Session 107 smoke test (2026-04-12) generated `SAMPLE-UGC-005` — a BGC High Street OOTD shot that was technically correct. It hit every variety bank pick verbatim (location, lighting, subject archetype, outfit, atmosphere, photographic treatment). But the output was a full-body street shot where the sunglasses occupied <10% of the frame and were visually unrecognizable. RA: *"we need to keep the UGC away from whole body shots, the sunglasses are barely recognizable."* The variety banks had no constraint preventing this. The `Photographic Treatment Bank` even included "wide environmental shot" and "low-angle hero from ground up" as allowed options, which guarantee the sunglasses won't be prominent.

**How to apply:**

UGC skill (`dubery-ugc-prompt-writer`) needs a hard rule:

- **R6 (new rule)**: Product must occupy enough of the frame that the lens shape and Dubery logo are clearly visible. Rule of thumb: if the product were hidden, the image should no longer make sense as a sunglasses post.
- **Ban from photographic treatment bank** (for person-anchor scenarios): "wide environmental shot", "low-angle hero from ground up"
- **Add tight-crop bank** to replace banned entries: "waist-up", "chest-up", "face + shoulders", "tight face crop", "3/4 profile head-and-shoulders", "over-the-shoulder focused on product"
- **Product-anchor UGC scenarios** (DASHBOARD_FLEX, CAFE_TABLE, GYM_BAG, etc.) don't need this rule — product is naturally the hero
- **DONE (session 112):** R6 applied as hard rule in `dubery-ugc-prompt-writer`. Banned "wide environmental shot" and "low-angle hero from ground up" from Photographic Treatment Bank. Added 6-option Framing Bank (waist-up, chest-up, face+shoulders, tight face, over-shoulder, side profile). Self-check and execution order updated.

**Edge case:** Environmental storytelling shots (e.g. "walking down BGC High Street") are still valuable but must be framed tight enough that the product reads. Candid back-camera shots from friends at medium distance are OK. Drone/wide-lens city compositions are not.
