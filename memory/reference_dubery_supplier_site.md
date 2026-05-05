---
name: Dubery Sunglasses Supplier Site
description: Original supplier website for Dubery sunglasses -- product specs, SKU mapping, and image library
type: reference
related: [project_chatbot_knowledge_base.md, feedback_visual_product_inspection.md]
---

**Supplier:** Dubery Sunglasses (duberysunglasses.com) -- the original brand RA resells as DuberyMNL in the Philippines.

**Product page URLs:**
- Bandits collection: https://duberysunglasses.com/collections/bandits/
- Outback collection: https://duberysunglasses.com/collections/outbacks/
- Rasta (D008): blocked by Alibaba CAPTCHA, specs from reseller chosmachai.com

**SKU Mapping (supplier -> RA's name):**

Bandits (D518):
- D518-01 "Bright Black/Black" = **Glossy Black**
- D518-02 "Black-Blue-Blue" = **Blue**
- D518-04 "Douhua-Tea" = **Tortoise**
- D518-05 "Sand-Black-Orange" = **Matte Black**
- D518-06 "Black-Green-Green" = **Green**

Outback (D918):
- D918-01 = **Black**
- D918-03 = **Red**
- D918-05 = **Green**
- D918-06 = **Blue**

Rasta (D008) -- same dimensions as D731 (146mm width, 58mm lens, 47mm height, 131mm temple, 31.6g)

**Specs (consistent across Bandits/Outback):**
- TR90 flexible frame
- Polycarbonate polarized lenses, 99.9% efficiency, UV400
- Scratch/shatter resistant, hydrophobic, anti-reflective inner coating
- ANSI Z80.3 rated
- Supplier price: $19.99 USD (all variants sold out on their site)
- Marketed for hiking, driving, beach

**Rasta difference:** PC frame (not TR90), TAC lenses (not polycarbonate), drawstring pouch + polarized tester included.

**Local supplier image library:** `references/supplier-images/` -- scraped 9 of 11 variants (Bandits x5, Outback x4). 80+ high-res studio shots. Useful as AI image gen references, not for customer-facing use (those are on Vercel + Drive).

**How to apply:** When a customer asks for technical specs, use these. When scraping new variants, use WebFetch + Bash curl (avoid Alibaba -- has CAPTCHA). When looking up a variant, remember the supplier's color names don't match RA's naming.
