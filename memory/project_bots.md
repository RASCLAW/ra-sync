---
name: Telegram Bots
description: Rasclaw and Belle bots — what they do, how they run, current status
type: project
---

**Rasclaw (@Rasclaw01_bot):** RA's personal Telegram bot. Command handlers + Claude Opus conversation mode + note-taking (log/note/remember/save commands). Also sends pre-shift briefs (ra_preshift_brief.py: systems check + Jah + money + projects).

**Belle (@arabelle01_bot):** Arabelle's personal Telegram assistant. Runs on Opus. Morning briefs (6:30 AM vacation, 10 AM normal days). Cron @reboot for auto-start + GitHub Actions fallback brief.

**Status (Apr 2026):** Both offline — need restart after PC rebuild. Bot scripts are in the DuberyMNL repo (rasclaw_bot.py, belle_bot.py or similar). Need .env keys (TELEGRAM_BOT_TOKEN for each) and process management.

**Why:** Rasclaw is RA's notification + quick-log channel. Belle is Arabelle's daily assistant. Both were running 24/7 before the PC died.

**How to apply:** When restarting, check that .env has the Telegram tokens, then start with process management (cron @reboot or similar). Future: Oracle Cloud VM for 24/7 uptime.
