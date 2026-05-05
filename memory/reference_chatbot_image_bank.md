---
name: Chatbot Image Bank v2
description: 48 chatbot images across 8 categories with per-image captions in a {url, caption} schema. Restored session 106 after session 101 shrank to 21.
type: reference
related: [project_chatbot_knowledge_base.md, project_chatbot_live.md, reference_cloud_run_chatbot.md, reference_cloudflare_migration.md, feedback_visual_product_inspection.md, project_image_bank_april.md]
originSessionId: b1211702-4f57-4a72-a5b5-98cb55c11c8f
---
**Current state (as of session 106, 2026-04-12):** 48 images, 8 categories, per-image captions, `{url, caption}` schema. Fully loaded in `cloud-run/knowledge_base.py`. Full image bank text is 65 lines in the chatbot system prompt.

**Schema:**
```python
PRODUCT_IMAGES = {
    "bandits-green": {
        "url": "https://duberymnl.com/assets/cards/bandits-green-card-shot.jpg",
        "caption": "Bandits Green -- flat-lay with Dubery box, pouch, cloth, warranty card. Green + black bicolor frame, blue-green mirror lenses, green/yellow tropical pattern on temples.",
    },
    ...
}
```

`get_image_url(key)` returns the URL string (backwards compatible with messenger_webhook.py). `get_image_caption(key)` returns the caption. `get_image_bank_text()` renders all 48 with per-image caption lines grouped by category, with a picking-guidance hint per category.

**8 categories, 48 images total:**
- **Hero shots (11)** -- product cards served from Vercel (`duberymnl.com/assets/cards/`). **Discovery session 106: every hero shot is a flat-lay with the full unboxing set** (Dubery box, drawstring pouch with microfiber cloth, blue warranty card on kraft/tan background). NOT clean product shots. Hero = default when customer asks what a variant looks like. **Hero also doubles as "what's in the box"** -- don't also send `support-inclusions` after sending a hero.
- **Model shots (6)** -- `model-*` -- on-face studio portraits. Served via lh3 CDN.
- **Lifestyle shots (6)** -- `lifestyle-*` -- real-environment mood shots (cafe, beach, river, campus). lh3 CDN.
- **Collection shots (4)** -- `collection-*` -- series showcases (all 5 Bandits together, all 4 Outback, both Rasta angles). **Previously banned as nonexistent in session 101 IMAGE RULES.** Session 106 restored them. lh3 CDN.
- **Brand graphics (5)** -- `brand-*` -- feature/benefit/typography (polarization, UV400, "made for the grind"). Use when explaining technical benefits. lh3 CDN.
- **Customer feedback (8)** -- `feedback-*` -- real Messenger review screenshots. Use when customer is hesitant or asks for reviews. lh3 CDN.
- **Proof shots (6)** -- `proof-*` -- shipping/stock legitimacy (COD packages, J&T pickup, LBC drop-off, inventory, branded boxes). Use when customer asks "is this legit?" or "do you have real stock?". lh3 CDN.
- **Sales support (2)** -- `support-inclusions` (standalone close-up, redundant to heroes but kept) + `support-instapay-qr` (provincial prepay).

**Picking guidance in IMAGE RULES** (conversation_engine.py, session 106 rewrite):
- Trust the caption. Lightly reference what the caption says (e.g. "here's Bandits Tortoise at a cafe") but never invent details beyond the caption.
- Lead with product description. Scene reference is optional and must come from the caption.
- Replaced the old session 98/101 rule "never describe the scene" -- that rule was right when Gemini was blind to image content, wrong now that captions describe scenes.

**Lazy Meta attachment caching:** First send of an image uploads it to Meta via `/me/message_attachments` and caches the `attachment_id`. Subsequent sends of the same image reuse the cached ID -- near-instant repeat sends. No startup warmup (session 101 removed the 48-image warmup after OOM). The chatbot can carry the full 48 without memory issues.

**Session history:**
- Session 97: basic hero shots only, no categories.
- Session 98: built full 48-image bank with 8 categories (commit d942c44). No per-image captions.
- Session 101: shrank 48 -> 21 as "over-correction, expansion parked for next session." Dropped model/collection/brand/feedback/proof categories entirely.
- Session 106 (current): restored 48, added per-image captions, refactored schema to `{url, caption}` dicts, fixed CATALOG variant_notes errors for 4 variants (Bandits Green/Tortoise, Outback Red/Green), encoded hero-doubles-as-inclusions into the category hint.

**Drive folder:** `DuberyMNL/Chatbot Images/` (ID: `1TnnaSmd_IzRbus3mCwYw--FO0k4pOByZ`). Manifest at `tools/chatbot/drive_image_manifest.json`. Upload script: `tools/drive/upload_chatbot_images.py` (idempotent, skips existing).

**How to add more images:** Drop new files into Drive, add entries to the manifest, re-run upload script. Then add a new entry to the corresponding category dict in `cloud-run/knowledge_base.py` with `{"url": _drive(file_id), "caption": "..."}`.

**TODO (session 114):**
- Replace hero shots with accurate, higher-quality versions (current ones are stale landing page card shots)
- Add a **worn shot** for every product variant (on-face, showing how it actually looks when worn) -- customers want to see how it looks on a person before buying
- Update duberymnl.com landing page assets to match (backlogged)
