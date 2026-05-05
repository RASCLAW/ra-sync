---
name: Claude Code Routines
description: Anthropic's cloud-hosted scheduled agents. 1hr min interval, 5-15-25 runs/day by plan tier. Does NOT replace sub-hourly cron or process launchers. First candidate = dashboard moderator.
type: reference
related:
  - ../../../../projects/EA-brain/references/summaries/nate-routines.md
  - ../../../../projects/EA-brain/references/summaries/brad-claude-code-usage-limits.md
  - reference_chatbot_autostart.md
  - reference_dashboard_moderator.md
  - feedback_claude_code_layers.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
**What Routines are:** Cloud-hosted scheduled Claude Code agents on Anthropic infra. Agent reasons, reads CLAUDE.md, self-corrects. No laptop needed.

**Trigger types:** schedule (cron-like), API webhook, GitHub event.

**Limits:**
- Minimum interval: 1 hour
- Runs per day: 5 (Pro) / 15 (Max 5x) / 25 (Max 20x)
- Each run burns session quota like interactive Claude Code

**What Routines are NOT:**
- NOT a process launcher — can't replace Windows Task Scheduler for chatbot autostart
- NOT sub-hourly — can't replace story rotation (12 images / 4 hours via GitHub Actions)
- NOT a Python script runner — scout.py / market_intel.py are overkill targets
- Likely redundant with existing `/schedule` skill (same Anthropic backend, different UI)

**Genuine candidate:** dashboard moderator (needs agent reasoning to sync data + trigger deploys periodically). Wire as first Routines task when moderator work begins.

**How to apply:**
- Use lean dedicated repo per routine (don't point Routines at full DuberyMNL — context cost compounds). Per Brad's usage-limits guidance.
- If task is deterministic (cron + Python), keep it on current cron. If task needs Claude reasoning, consider Routines.
- Don't migrate working cron jobs just because Routines exist.
