---
name: Live Chatbot Code Path
description: Live chatbot runs from chatbot/ (project root) not tools/chatbot/. Any live pricing/KB/prompt edits must target chatbot/. Renamed from cloud-run/ in session 127 (2026-04-16).
type: reference
related:
  - project_chatbot_recovery_complete.md
  - reference_cloudflare_worker_fallback.md
  - reference_chatbot_autostart.md
originSessionId: dcf22698-e101-4a70-8d5e-c94d2da0ed02
---
**Live path: `chatbot/`** (Flask on :8080, served via Cloudflare tunnel).
**Historical/deprecated path: `tools/chatbot/`** (old Cloud Run version from before the migration).

Both directories contain near-identical file trees (`knowledge_base.py`, `conversation_engine.py`, `messenger_webhook.py`, etc.). The tools/chatbot/ copies are stale — edits there don't affect production.

**Key files to edit when the bot's behavior needs to change:**
- [chatbot/knowledge_base.py](chatbot/knowledge_base.py) — CATALOG, PRICING, DISCOUNT_CODES, FAQ, IMAGE_BANK, LINKS. Source of truth for system prompt injection.
- [chatbot/conversation_engine.py](chatbot/conversation_engine.py) — SYSTEM_PROMPT (security, voice, formatting, first-message rules, inline pricing examples, JSON schema, extraction rules).
- [chatbot/messenger_webhook.py](chatbot/messenger_webhook.py) — Flask routes, /webhook, /chat-test, /status, warmup, stats.

**How to verify which file is live:**
```bash
curl https://chatbot.duberymnl.com/status
```
Then check the PID of python.exe running messenger_webhook.py — its working directory is `chatbot/` (formerly `cloud-run/` prior to 2026-04-16 rename).

**Why:** Session 122 (2026-04-15) pricing shift (DUBERY50 → bundle push at P599/P1099) initially edited only tools/chatbot/ and had no customer-facing effect. The fix required targeting chatbot/. Keep this distinction top of mind for any chatbot task.

**How to apply:** Default to editing chatbot/ for anything customer-facing. Touch tools/chatbot/ only if RA explicitly asks OR if you're working on something clearly not live (voice_chat.html, test_web.py). If you're unsure, `grep` both paths and edit both to stay consistent.
