---
name: tg-409-diagnosis-2026-05-21
description: "Confirmed Claude Code TG plugin (PID 6520 from Startup folder start-rasclaw.bat) is the long-poll winner; chatbot/monitor.py loses with 409 every 60s. Ongoing since 2026-05-15, 5283 backoff warnings logged, harmless per docs."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - feedback_tg_poll_409_claude_plugin.md
    - project_command_center_phase1_complete.md
    - project_rasclaw_telegram.md
  originSessionId: 9b138beb-b18d-43b3-8344-a001cda5665d
---

## What's happening
Two processes share `TELEGRAM_BOT_TOKEN` (→ `@Rasclaw01_bot`) and both want a Telegram long-poll. Telegram only allows one:

- **Winner:** PID 6520 `claude.exe --channels plugin:telegram@claude-plugins-official`, spawned at logon by `~/AppData/Roaming/Microsoft/Windows/Start Menu/Programs/Startup/start-rasclaw.bat` (a copy of `Rasclaw/scripts/start-rasclaw.bat`). This is RA's "talk to Rasclaw from my phone" channel.
- **Loser:** PID 12384 `chatbot/monitor.py`, spawned by Task Scheduler entry `\DuberyMNL-Chatbot` at logon via `chatbot/start-monitor.bat`. Loses `getUpdates` with 409, sleeps 60s, retries.

## Scale
- First 409: 2026-05-15 10:11:41 (6 days running)
- Total 409 warnings: 5,283 (in `chatbot/.tmp/monitor.log`)
- Cadence: ~1/min, exactly the documented 60s backoff
- RA noticed today only — cmd window scroll buffer

## Confirmed not-a-cause
- Telegram webhook is empty (`getWebhookInfo` returned `"url": ""`), so it's not a webhook-vs-polling conflict
- No second monitor.py instance, no stale duplicate from boot.bat re-run
- `command-center/monitors/chatbot_tg.py` only does a one-shot `getMe` healthcheck — not a poll
- Rasclaw repo has no Python TG bot; its runtime is the Claude Code plugin + Cloudflare Worker

## Effect (per existing memory feedback_tg_poll_409_claude_plugin.md)
- ✅ TG **sends** (crash notifications, /mark-sale alerts) still work — they don't conflict
- ✅ Auto-restart + crash detection still work — health loop is HTTP, not TG
- ❌ `/restart` and `/status` Telegram commands to monitor.py are unreachable while Claude Code is running (which is ~always)
- ❌ Cmd window log spam: 1 WARNING line per minute

## Why it's harmless
monitor.py's 409 branch sleeps cleanly; no spin, no CPU burn, no crash. The `/restart` and `/status` commands were nice-to-haves — monitor.py auto-restarts the chatbot via the HTTP health loop regardless.

## Architectural fix options
1. **Status quo** — accept 1 WARN/min, lose `/restart` + `/status` commands
2. **Quiet the monitor** — extend backoff after N consecutive 409s (e.g., 60s → 5min after 5 in a row), keep token shared. Cheap, ~5 min code change.
3. **Separate bot token** — create a second BotFather bot, store as `MONITOR_BOT_TOKEN`, point monitor.py at it. Restores `/restart` + `/status` even with Claude Code open. ~10 min including BotFather step.
4. **Drop monitor.py TG poll** — remove `tg_poll_loop()` thread entirely, keep only sends. Eliminates noise + simplifies code. Lose `/restart` + `/status` permanently.

**Resolution:** RA picked option 4 after confirming `/restart` and `/status` were unused (CC Monitor tab + the HTTP health-loop auto-restart already cover both). Implemented in DuberyMNL commit `301820d`:
- Removed `tg_poll_loop()`, `tg_reply()`, `RA_CHAT_ID` from `chatbot/monitor.py`
- Removed `/restart` + `/status` table from `chatbot/README.md`
- Killed old monitor (PID 12384) + orphaned chatbot (PID 15420), relaunched via `start-monitor.bat` -> new PIDs 17036 + 4032
- No more 409 warnings in monitor.log post-restart
- TG sends (crash pings, /mark-sale alerts) still work — they never conflicted

Token-sharing is now single-poller: only the Rasclaw plugin polls `@Rasclaw01_bot`. The Cloudflare Worker only sends. monitor.py only sends.
