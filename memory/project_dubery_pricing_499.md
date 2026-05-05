---
name: DuberyMNL Pricing — 499/pair
description: Current pricing locked at 499/pair, free shipping on 2+, provincial pre-pay only
type: project
related: [project_chatbot_knowledge_base.md, project_messenger_strategy.md]
originSessionId: b1aea53b-bbbc-4ae7-a895-504a630a8a04
---
Pricing changed 2026-04-25 (session 143). Previous price was 599/pair.

**Current pricing:**
- Single pair: 499 + shipping fee (varies by address, ~99 MM)
- 2+ pairs: 499/pair, FREE shipping nationwide
- No per-pair bundle discount — free shipping IS the bundle benefit
- COD: Metro Manila only
- Provincial: prepaid only (GCash, bank transfer, InstaPay)

**Files updated:**
- `chatbot/knowledge_base.py` — PRICING dict + FAQ answers + get_pricing_text()
- `chatbot/cloudflare-worker/worker.js` — FAQ templates (pricing + shipping), redeployed ID da28c30d
- `dubery-landing-v3/products/data.json` — all 11 products
- `dubery-landing-v3/order/order.js` — removed 99 bundle discount, delivery=0 on 2+
- All v3 HTML pages: trust strip, hero, best-sellers, story section, CTA banner, PDP

**Why:** Price drop to compete and drive volume. Simplified structure removes "99 discount" complexity.

**How to apply:** Any content, chatbot reply, or landing page copy referencing price should use 499. Bundle messaging = "free shipping on 2+", not "save 99".
