---
name: TG Poll 409 — Claude Code Plugin Conflict
description: Claude Code's Telegram plugin long-polls the same bot token; causes persistent 409 on monitor.py getUpdates; commands only work when Claude Code is closed
type: feedback
related: [reference_tg_send_helper.md, feedback_chatbot_architecture.md]
originSessionId: 0ef80e2f-49fd-4996-b6ef-fd3e0f504429
---
Claude Code's Telegram plugin (`~/.claude/plugins/cache/claude-plugins-official/telegram/0.0.6`, runs as `bun server.ts`, PID visible in process list) holds a long-poll `getUpdates` on the same bot token used by DuberyMNL chatbot and monitor.py.

When both are running simultaneously, monitor.py's `tg_poll_loop` gets a persistent `HTTP 409 Conflict` response.

**Why:** Telegram Bot API only allows one concurrent `getUpdates` caller per token. Claude Code plugin wins the poll while open.

**How to apply:**
- `monitor.py` has a 60s backoff on 409 — it will keep retrying but won't spin
- TG crash notifications (sends) are unaffected — only polling is blocked
- `/restart` and `/status` Telegram commands only work when Claude Code is closed
- Auto-restart + crash detection still work regardless (health loop is HTTP, not TG poll)
- If you need TG commands while Claude Code is open, would need a separate dedicated bot token for monitor.py
