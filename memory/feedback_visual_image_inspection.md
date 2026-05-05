---
name: Visual Inspection for Ambiguous Filenames
description: Filenames alone misclassify — pHash-16 matching or visual review required for images that don't clearly indicate model/type (multiref_*, image_*, test-*, V3-*, SAMPLE-*, CAT*, OB_*, plaintext_*)
type: feedback
related: [feedback_visual_product_inspection.md, reference_review_dashboard.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
Ambiguous-named images need visual inspection to classify model + type (person vs product). Filename keywords alone give wrong answers for ~15-30% of reviewed content. Diagnosed session 126 while reorganizing `contents/ready/` into `person/{model}/` + `product/{model}/`.

**Why:** Examples from session 126:
- `multiref_bandits_blue.png` — filename says "product/multiref" but actually a PERSON shot (woman wearing)
- `C3_fidelity_beach.png` — no model hint, turned out to be outback-blue product
- `image_c0741e0d.png` / `image_e636a1f6.png` / `image_e64c2a80.png` — random UUIDs, two were rasta-brown PRODUCT, one was bandits-green PERSON
- `test-red-51/52/54/55/56` — person, but `test-red-53` is product. Same prefix, different types.
- `CAT2c_ugc_headband` — person shot even though `ugc` filename was classified as product by earlier heuristics

**How to apply:**
- For well-named generations (`bandits-green-02-flatlay.png`), trust the filename — fast classifier works.
- For ambiguous names, view the actual image. Claude's Read tool is multimodal — use it.
- For bulk matching against a known set (e.g. 11 hero shots), use `imagehash.phash(..., hash_size=16)` with Hamming distance — 16-bit resolves the collisions that 8-bit missed. Session 126 had 3 images with identical 8-bit phash (kraft bg + dark sunglasses) that 16-bit separated cleanly.
- Maintain a `VISUAL_OVERRIDES` dict in reorg scripts mapping filename → (type, model) for persistent ambiguous cases.

**Related:** `feedback_visual_product_inspection.md` (RA's broader rule to Read hero shots before writing copy) — this is the same principle applied to bulk reorg.
