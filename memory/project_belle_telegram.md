---
name: Belle Telegram Channel
description: PLANNED -- Telegram EA for Arabelle using Belle bot, same channel plugin pattern as Rasclaw
type: project
related: project_rasclaw_telegram.md, user_arabelle.md, project_oracle_migration.md
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
## Status: PLANNED

Same architecture as Rasclaw but for Arabelle:
- Bot: @arabelle01_bot (BELLE_BOT_TOKEN in .env)
- Separate TELEGRAM_STATE_DIR to avoid collision with Rasclaw
- Separate start-belle.bat in Startup folder
- Separate system prompt with Belle's personality
- Her own project folder (~/projects/Belle/) with context, memory, preferences

## Belle's context needs
- Family dashboard
- Calendar/schedule (work: WFH Mon+Fri, office Tue-Thu, 6am-3pm)
- Zach (school, activities)
- Baby Jah
- Budget/finance
- HMO (Maxicare)
- Mama Rowena health monitoring

## Key difference from Rasclaw
No dev tools, no coding. Personal assistant focused on family, health, schedule, reminders.

**Why:** RA wants to build a personal EA for Arabelle as well, with her own isolated context and memory.
**How to apply:** Follow Rasclaw setup pattern but with TELEGRAM_STATE_DIR isolation and Belle-specific content.
