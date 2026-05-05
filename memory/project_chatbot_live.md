---
name: Chatbot Live on Cloud Run
description: DuberyMNL Messenger chatbot deployed to Cloud Run -- architecture, URL, what works, what's pending
type: project
related:
  - reference_cloud_run_chatbot.md
  - project_messenger_strategy.md
  - feedback_chatbot_language.md
  - reference_cloud_run_cpu_throttle.md
  - project_oracle_migration.md
  - reference_ph_customer_shorthand.md
  - reference_cloudflare_migration.md
  - reference_chatbot_image_bank.md
originSessionId: 2d42edda-47ed-453a-8685-08619336342c
---
## Status: LIVE (session 117, 2026-04-13). Full recovery complete.

**Live URL**: `https://chatbot.duberymnl.com/webhook`
**Architecture**: local Flask:8080 -> Cloudflare named tunnel -> chatbot.duberymnl.com, with Worker fallback for offline
**Model**: Gemini 2.5 Flash via Vertex AI REST API, location `global`
**See also**: `project_chatbot_recovery_complete.md` for full recovery checklist

## Architecture
- Single Cloud Run service, synchronous processing (no background threads)
- Webhook receives Meta event -> calls Gemini -> sends reply via Send API -> returns 200
- In-memory conversation store (resets on instance restart)
- Comment auto-responder integrated but untested

## What Works
- Messenger webhook receiving messages
- Gemini generating replies (~3 seconds)
- Send API delivering replies to Messenger
- Typing indicator
- Conversation history within session
- Handoff system (emails RA)

## Features
- Conversation history (in-memory, resets on instance restart)
- Handoff system (emails RA when order complete, complaint, or bot unsure)
- Image sending (kraft card product photos from duberymnl.com)
- Message dedup (skips Meta retries by message ID)
- Comment auto-responder (integrated but untested)

## Pending
- `pages_messaging` App Review (needed for non-admin users)
- Comment auto-responder testing
- Website chatbot widget (same backend, new endpoint)
- Chat history database (Sheets first for CRM, then Supabase for portfolio)
- Lead scoring system (hot/warm/cold/converted)
- Telegram handoff ping (via Rasclaw) instead of email

## Session 98 Updates (2026-04-10)
- Full knowledge base rebuild -- accurate product descriptions, new persona, specs added
- 48 images wired across 8 categories (see reference_chatbot_image_bank.md)
- Persona: warm and direct (not jolly), says "I", short responses, 95% English
- Order flow: adds landmarks + delivery preference + urgent handling
- Provincial orders: prepaid only, bot sends InstaPay QR
- DUBERY50: only mentioned when customer brings it up (was: proactive)
- Tagline changed: "Premium polarized shades at everyday prices"
- Inclusions corrected: drawstring pouch, not hard case (hard case is +P100 optional)

## Key Bugs Found During Setup
1. Wrong token: `META_ADS_ACCESS_TOKEN` doesn't work for Send API, must use `META_PAGE_ACCESS_TOKEN`
2. `google-genai` SDK hangs on Cloud Run -- switched to direct REST API
3. `gemini-2.0-flash` returns 404 (restricted/sunset) -- use `gemini-2.5-flash`
4. Background threads die silently on Cloud Run -- switched to synchronous inline processing
5. `gunicorn --timeout 30` kills workers -- must use `--timeout 0` on Cloud Run
6. `min-instances=1` required to keep instance alive between requests
7. **Deadlock**: ConversationStore used threading.Lock (not reentrant) -- append_message called get_or_create while holding lock. Fixed with RLock.
