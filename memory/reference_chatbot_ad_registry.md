---
name: Chatbot Ad Registry
description: Maps Meta Click-to-Messenger `ref` tags (or ad_id fallback) to per-ad opener hints so the bot tailors first replies to the specific ad the customer came from. Edit chatbot/ad_registry.json to add/change entries.
type: reference
related:
  - project_chatbot_employee_discipline.md
  - reference_chatbot_admin_endpoints.md
  - reference_chatbot_live_path.md
originSessionId: f2b83343-be79-40bb-b8a6-851c6856aee3
---
File: `chatbot/ad_registry.json`. Loaded lazily on first use, cached in-process. Restart or wait for cache eviction to pick up edits.

**Schema:**
```json
{
  "<REF_OR_AD_ID>": {
    "product_focus": "<label or null>",
    "opener_hint": "<instruction for Gemini on how to open>"
  }
}
```

**Three categories of entries:**

| Category | When to tag the ad | Example keys |
|---|---|---|
| Per-variant | Single-product ad, or one slide of a carousel (each slide gets its own ref) | `BANDITS_TORTOISE`, `OUTBACK_RED`, `RASTA_BROWN` |
| Per-series | Single-image ad showing the whole model line (no per-slide granularity) | `BANDITS_SERIES`, `OUTBACK_SERIES`, `RASTA_SERIES` |
| Generic | Price/sale-driven or mixed-catalog ads | `PRICING_SALE`, `COLLECTION_HERO`, `FULL_CATALOG` |

**Meta Ads Manager tagging:**
- Ad setup → Messenger destination → JSON payload
- Set `{"ref": "BANDITS_TORTOISE"}` (or whatever tag)
- Carousel ads: expand each slide and set its own ref

**How the flow works end-to-end:**
1. Customer clicks tagged ad → Meta fires `referral` webhook BEFORE first user message
2. `chatbot/messenger_webhook.py` stamps `source_ref` / `source_ad_id` on conversation metadata + logs to `.tmp/referral_log.jsonl`
3. First customer message → `run_generate` pulls ad context via `get_ad_context(source_ref, source_ad_id)`
4. Match found → `opener_hint` + `product_focus` injected into Gemini's dynamic system-prompt context block
5. Gemini opens with ad-aware reply (e.g. "Saw you checking out the Bandits Tortoise — tortoise frame, polarized. ₱599...") instead of generic sales template
6. Turn 2+: hint is skipped (first-contact only)

**Fallback safety:** unknown `ref` or `ad_id` returns None → Gemini uses generic SALES TEMPLATE. Nothing breaks when ads aren't tagged.

**How to apply:**
- Before suggesting an ad-aware behavior tweak, check the current registry (15 entries as of session 127).
- When RA ships a new ad variant, suggest adding a matching registry entry in the same session.
- If RA reports "the bot isn't opening with product context" -- first confirm the ad itself has a `ref` field set (common miss), THEN confirm the tag exists in `ad_registry.json`.
