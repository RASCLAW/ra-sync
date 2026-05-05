---
name: Meta Messenger Attachment Caching
description: Pre-upload images to Meta for reusable attachment IDs -- fast repeat sends via Send API
type: reference
related: [project_chatbot_live.md, reference_chatbot_image_bank.md, reference_cloud_run_chatbot.md]
---

Messenger's Send API supports two image delivery modes:

**1. URL-based (slow path):**
Send `{"attachment": {"type": "image", "payload": {"url": "...", "is_reusable": true}}}`. Meta fetches the URL each time, adds latency, and shows a loading circle in the user's chat.

**2. Attachment ID (fast path):**
Pre-upload via `POST /v21.0/me/message_attachments` with the URL payload + `is_reusable: true`. Meta returns an `attachment_id`. Subsequent sends use `{"attachment": {"type": "image", "payload": {"attachment_id": "..."}}}`. No URL fetch, near-instant delivery.

**Implementation in DuberyMNL chatbot:**
- In-memory dict `_attachment_cache: {url -> attachment_id}` in `cloud-run/messenger_webhook.py`
- `send_image()` checks cache first, falls back to upload + cache on miss
- `warm_attachment_cache()` runs at module load to pre-upload all 48 images from `knowledge_base.ALL_IMAGES` (takes ~30-60s but eliminates first-send loading circles)
- Cache dies on Cloud Run instance restart -- warmup re-runs on next boot

**Why:** First customer to see an image via URL path had a 2-3s loading circle (especially for 1.4MB PNGs). With attachment_id caching + startup warmup, every send is near-instant because Meta already has the image.

**How to apply:** Any Messenger bot sending repeatable assets (product photos, stock images, branding) should use this pattern. Free via Meta API, no external CDN needed. Remember to refresh cache after deploy since Cloud Run instances restart.
