---
name: Google API cache_discovery=False Hangs on Restart
description: cache_discovery=False forces a network fetch of the discovery doc every restart — no timeout, hangs indefinitely; pre-warm at startup instead
type: feedback
related: [feedback_crm_adc_scope.md, feedback_chatbot_architecture.md]
originSessionId: 6301781d-2f22-4e59-b1f1-3a4187f641a5
---
Never use `cache_discovery=False` in production Google API `build()` calls. Pre-warm the service client at startup in a daemon thread.

**Why:** `googleapiclient.discovery.build("sheets", "v4", cache_discovery=False)` fetches the discovery document from Google's API on every call. On RA's home network this fetch sometimes hangs indefinitely (no socket timeout in `httplib2` by default). After each chatbot restart, the first customer message triggered a lazy `_get_service()` → `build()` → hang. Bot appeared to receive the message but never replied.

Fix applied in `crm_sync.py` (removed `cache_discovery=False`) and `messenger_webhook.py` (`_crm_prewarm` thread in `_start_warmup_once()` — builds the Sheets service at startup alongside image warmup so it's ready before the first message arrives).

**How to apply:** For any new Google API client in this project: use default `cache_discovery` (disk cache), and pre-warm the service at startup in a background thread if the first call latency matters.
