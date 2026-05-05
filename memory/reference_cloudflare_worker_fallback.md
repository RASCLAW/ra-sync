---
name: Cloudflare Worker Fallback
description: dubery-chatbot-fallback Worker -- FAQ layer + KV dedup when Flask is down, 🚨 TG ping only on order_intent
type: reference
related:
  - reference_cloudflare_migration.md
  - project_chatbot_recovery_complete.md
  - reference_chatbot_live_path.md
  - reference_chatbot_autostart.md
  - feedback_worker_ping_rule.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Worker:** `dubery-chatbot-fallback`
**Route:** `chatbot.duberymnl.com/*`
**Deployed via:** `cd cloud-run/cloudflare-worker && npx wrangler deploy`
**Cloudflare account:** `sarinasmedia+rasclaw@gmail.com`

**How it works (session 125 rewrite):**
1. All requests to chatbot.duberymnl.com hit the Worker first
2. Worker forwards to origin (Cloudflare tunnel → Flask)
3. If origin responds < 500, pass through transparently
4. If origin is down, Worker classifies the customer message by intent:

| Intent | Triggers | Customer sees | TG ping |
|---|---|---|---|
| `order_intent` | phone (09xxxxxxxxx) + address keyword | "Got it po 🙏 owner will reach out..." | 🚨 URGENT |
| `pricing` | "hm", "how much", "magkano", "price", standalone "how", "?" | pricing template + album link + disclaimer | silent |
| `polarized` | "polarized", "polarize", "pola" | polarized template + disclaimer | silent |
| `shipping` | "ship", "sf", "delivery fee" | shipping template + disclaimer | silent |
| `how_to_order` | "how to order", "paano mag order", "order po" | 7-field form + disclaimer | silent |
| no match + recent FAQ | (follow-up after canned reply) | silent (suppress polite hold) | silent |
| no match fresh | "Hi", "legit ba?", etc. | polite hold message | silent |

**Disclaimer footer:** "(This is quick auto-reply po — owner will follow up for anything else soon 🙏)"

**KV dedup:** `FAQ_DEDUP` namespace, 10-min TTL per `faq:{senderId}:{intent}`. Same intent within 10min = silent. Order_intent bypasses dedup.

**Secrets:** `PAGE_ACCESS_TOKEN`, `TELEGRAM_BOT_TOKEN` (via `npx wrangler secret put`)
**Env vars (wrangler.toml):** `VERIFY_TOKEN`, `FALLBACK_MESSAGE`, `TG_CHAT_ID`
**KV binding:** `FAQ_DEDUP` (id `3ff16e193cd2431eb770cd3bab232f58`)

**Files:** `cloud-run/cloudflare-worker/worker.js` + `wrangler.toml`
**Unit test:** `cloud-run/cloudflare-worker/test-classifier.mjs` (34 scenarios)
