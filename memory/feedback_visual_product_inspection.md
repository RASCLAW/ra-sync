---
name: Visually Inspect Products Before Writing Copy OR Captions
description: Always read the actual product images before writing descriptions, captions, or copy. Don't trust supplier text, existing memory, or gestalt visual impression on comparisons.
type: feedback
related: [project_chatbot_knowledge_base.md, feedback_chatbot_architecture.md, reference_chatbot_image_bank.md, feedback_visual_image_inspection.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
When writing ANY product content for DuberyMNL -- descriptions, captions, or marketing copy -- always Read() the actual images first. Do not trust supplier descriptions, existing knowledge base text, memory, or assumptions.

**This applies to captions too, not just long-form copy.** Session 106 wrote 48 chatbot image captions from memory and filenames without reading a single image. Multiple were wrong.

**Why -- track record of errors this rule catches:**

*Session 98:* Described Bandits as "sporty wraparound frame" (copied from supplier text). Actual frame = slim, clean square with chrome temple badge. RA had to correct me twice. Direct feedback: "why dont you look at the sunglasses so you know hehe."

*Session 106:*
1. Wrote all 11 hero captions as "clean product shots on white" without reading them. Actual shots = flat-lay on kraft background with Dubery box + pouch + cloth + warranty card. Completely different framing implication.
2. Inherited CATALOG errors from session 98 "visually verified" text: Outback Red was said to have gold/amber mirror lenses (actually red/orange), Outback Green was said to have green-blue mirror lenses (actually green/purple iridescent), Bandits Green was said to be black frame with green accents (actually green + black bicolor), Bandits Tortoise was said to be dark tortoiseshell (actually brown + dark brown dominant). **Even memories that claim "verified" may need re-verification.**

**Anchoring bias warning -- visual comparison edition:**

When comparing two images for similarity, do NOT trust gestalt impression. Force yourself to check specific geometric features on a second pass: edge curvature, relative size, proportions, temple/arm decoration, lens height. Session 106 I claimed Rasta and Outback hero shots had "the same square frame shape" based on first-pass gestalt. RA pushed back. Second look: Rasta has a curved top edge (aviator-influenced), visibly wider frame, taller lens height, different temple decoration. The CATALOG "oversized aviator-style square frame" description was correct; my first-pass comparison was wrong.

Lesson: when doing visual A/B, read the images TWICE. First pass = "what do I see?" Second pass = "let me cycle through a feature checklist before drawing conclusions." Don't commit to a verdict from the first glance.

**How to apply:**
- Before writing any product copy or image caption: Read() the hero shots from `dubery-landing/assets/cards/` or `references/supplier-images/` directly.
- When captioning many images in batch: do them with parallel Read() calls, not from memory and filename guessing.
- When comparing two images: force yourself through a feature checklist (edge curvature, relative size, temple detail, lens height) on the second pass before drawing conclusions.
- When a memory claims "visually verified" -- re-verify on contact if the content is product-specific color/shape details. Session 98's claim was partially wrong; session 106 caught it.
