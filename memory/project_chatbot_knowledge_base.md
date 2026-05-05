---
name: Chatbot Knowledge Base Rebuild
description: Full rebuild of chatbot knowledge base with accurate product descriptions, persona, image bank, and order flow. Session 106 v2 updates for CATALOG fixes + flat-lay discovery.
type: project
related: [project_chatbot_live.md, feedback_chatbot_language.md, feedback_visual_product_inspection.md, reference_cloud_run_chatbot.md, reference_chatbot_image_bank.md, reference_cloudflare_migration.md]
originSessionId: b1aea53b-bbbc-4ae7-a895-504a630a8a04
---
Session 98 (2026-04-10) -- RA asked to rebuild the chatbot knowledge base from draft state to production-ready.

**Source of truth:** `tools/chatbot/KNOWLEDGE_BASE.md` -- editable markdown document RA can update freely. Synced to `cloud-run/knowledge_base.py` when finalized.

**Key content changes from draft:**
- **Product descriptions:** Rewrote from actual visual inspection of hero shots. Bandits = slim square with chrome badge. Outback = blocky angular with colored badge. Rasta = oversized aviator with gold accents + rasta stripe. Per-variant notes (e.g. Matte Black pattern is inside temples only).
- **Specs:** Added TR90/PC frame materials, dimensions (146mm width, 58mm lens, 47mm height, 131mm temple), weight (31.6g), polarization (99.9%), ANSI Z80.3.
- **Pricing:** Metro Manila delivery ~P99 single / FREE bundle. Provincial = no COD, GCash/bank transfer/InstaPay only.
- **Inclusions:** Drawstring pouch (NOT hard case). Hard case is optional +P100.
- **DUBERY50:** P50 off FIRST ORDER only. Bot mentions only when customer brings it up -- do NOT offer proactively.
- **Tagline:** "Premium polarized shades at everyday prices" (replaced "don't break the bank").
- **Order flow:** name, address, LANDMARKS, phone, model+color, delivery preference (same/next/urgent), time.
- **Urgent orders:** Bot asks for customer's number and says "I'll call ASAP" -- never gives out RA's number.
- **Returns:** Quality-checked before delivery (don't lead with return policy).

**Persona changes:**
- "Warm and direct" (not jolly, not "smart friend")
- Short responses by default, match customer energy
- Says "I" not "we"
- 95% English with casual Filipino sprinkles
- 24/7 operation

**Handoff:** "I'll have the owner message you shortly" + Telegram ping (via Rasclaw).

**Pricing (updated 2026-04-25):** 499/pair (was 599). Free shipping on 2+. Single pair + shipping fee. See `project_dubery_pricing_499.md`.

**Provincial blocker:** RA can't ship COD outside Metro Manila because Shopee/J&T require business documents he doesn't have yet (only DTI certificate). Prepaid provincial via GCash/InstaPay is fine.

**Why:** The draft knowledge base had inaccurate product descriptions, wrong inclusions (hard case instead of pouch), missing provincial payment flow, outdated persona, and no image support beyond basic hero shots.

**How to apply:** `KNOWLEDGE_BASE.md` is the editable source. When content needs updating, edit the markdown first, then sync to `cloud-run/knowledge_base.py`. Don't edit the Python file directly for content changes.

---

## Session 106 Updates (2026-04-12)

**CATALOG variant_notes errors fixed** (session 98 "rewrote from actual visual inspection" claim was partially wrong -- 4 variants had errors inherited from supplier text):
- Outback Red: `gold/amber mirror lenses` -> `red/orange mirror lenses`
- Outback Green: `green-blue mirror lenses` -> `green/purple iridescent mirror lenses`
- Bandits Green: `black frame with green accents` -> `green + black bicolor frame`
- Bandits Tortoise: `dark tortoiseshell (black + red/brown)` -> `brown + dark brown tortoiseshell pattern`

**Hero shots are flat-lay with full unboxing set.** Discovery via visual read of all 11 hero cards. Every shot shows the sunglasses alongside the Dubery box + drawstring pouch + microfiber cloth + warranty card on a kraft/tan background. NOT "clean product shots on white." All 11 hero captions rewritten to lead with the flat-lay context. Encoded into the hero category hint: "don't also send support-inclusions after a hero" -- prevents redundant double-sends since hero shots already show inclusions.

**Image bank schema refactored** to `{url, caption}` dicts per image (was bare URL strings). Each image now carries a per-image caption so Gemini can pick the right one for conversational context. See `reference_chatbot_image_bank.md` for full schema + category breakdown.

**Conversation engine IMAGE RULES rewritten:**
- Removed "collection-/comparison- don't exist" ban (restored collection category)
- Replaced "never describe the scene" rule with "trust caption, don't invent beyond"
- Added category-by-category picking guidance (when to use feedback/proof/brand/model/lifestyle shots)
