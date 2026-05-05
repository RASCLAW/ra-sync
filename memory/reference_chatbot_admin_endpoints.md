---
name: Chatbot Admin Endpoints
description: Local-only HTTP endpoints for the running DuberyMNL chatbot -- flag/release conversations, health checks, manual test chat, admin dashboard.
type: reference
related:
  - project_chatbot_employee_discipline.md
  - reference_chatbot_live_path.md
  - reference_chatbot_autostart.md
originSessionId: f2b83343-be79-40bb-b8a6-851c6856aee3
---
The Flask chatbot at `http://127.0.0.1:8080` (Cloudflare tunneled to `chatbot.duberymnl.com`) exposes these routes for RA's own use. All are on the local origin only — the tunnel forwards `/webhook` externally, but admin routes stay LAN.

**Runtime control**
- `POST|GET /flag/<sender_id>?reason=<label>` — manually flag a conversation for handoff. Bot goes silent. `reason` defaults to `human_takeover`. Use when you want to take over proactively before the bot replies again.
- `POST|GET /release/<sender_id>` — clear handoff, bot resumes replying. No-ops if not flagged.
- `POST|GET /mark-sale/<sender_id>` — record a manual-close sale into CRM. Required: `items`, `total`. Optional: `quantity`, `payment_method` (default COD), `delivery_preference`, `delivery_time`, `discount_code`, `name`/`phone`/`address`/`landmarks` (triggers upsert_lead), `note` (custom transcript line), `force=true` (override duplicate guard), `flag_handoff=false` (keep bot active). Accepts JSON body, form data, or query string. Side effects: creates CRM order row, stamps `order_recorded` + `last_order_id`, flags handoff, appends transcript note, resets reply-signature FIFO.

**Health**
- `GET /status` — JSON with `stats` (`messages_received`, `messages_sent`, `handoffs_triggered`, plus counters added session 127: `loop_handoffs`, `turn_cap_handoffs`, `human_takeovers`, `referrals_captured`), `started_at`, `warmup_complete`.
- `GET /readiness` — 200 once Meta attachment warmup finishes (~60-90s), 503 while warming. Cloud Run probe style.

**Local test**
- `GET /chat-test` — browser chat UI, bypasses Meta entirely. Session IDs auto-prefixed `TEST_`.
- `POST /chat-test` — JSON `{"message": "...", "session_id": "TEST_...", "customer_name": "..."}` to script tests. Only exercises `run_generate` (pre-Gemini gates), NOT `process_message` (post-Gemini orchestration).

**Admin view**
- `GET /conversations` — HTML table of last 20 conversations with messages, handoff status, intents.

**Meta webhook**
- `GET /webhook` — verification challenge for Meta App setup.
- `POST /webhook` — incoming Messenger events (HMAC verified against `META_APP_SECRET`).

**Env vars referenced by the above (all in `.env`):**
- `META_PAGE_ACCESS_TOKEN`, `META_PAGE_ID`, `META_APP_SECRET`, `META_APP_ID`, `MESSENGER_VERIFY_TOKEN`
- `TELEGRAM_BOT_TOKEN`, `TG_CHAT_ID`
- `CHATBOT_TURN_CAP` (default 10) — employee-discipline turn cap

**How to apply:** when RA asks to pause/resume a specific customer, use `/flag` or `/release` with the PSID from `/conversations` rather than editing `conversation_store.json` directly (which mis-syncs with the running process's in-memory state).
