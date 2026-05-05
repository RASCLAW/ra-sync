---
name: Telegram Send Helper Script
description: Reusable Python CLI for sending text/photo/file to RA's Telegram via Rasclaw bot without permission prompts
type: reference
related: reference_telegram_send_patterns.md, project_rasclaw_telegram.md
originSessionId: 0e9083ce-914e-4831-9771-f4ffb1686e6d
---
Location: `~/.claude/scripts/tg-send.py`

Usage:
```bash
python ~/.claude/scripts/tg-send.py text "message" [--html]
python ~/.claude/scripts/tg-send.py photo /path/to/image.png "caption"
python ~/.claude/scripts/tg-send.py file /path/to/file.json "label"   # sends file contents as <pre> code block
```

**Auto-allow:** `.claude/settings.local.json` in DuberyMNL allows `Bash(python ~/.claude/scripts/tg-send.py:*)` and `Bash(python C:/Users/RAS/.claude/scripts/tg-send.py:*)` without prompting.

**Why:** Session 120 -- RA wanted TG sends without permission prompts. Previous pattern was an inline bash heredoc with urllib calls; the helper replaces that with a clean CLI that can be pattern-matched in permissions.

**How to apply:** Use for any mid-session image/prompt review where RA wants to see on mobile. Respects HTML parse_mode, handles multipart photo uploads, truncates file contents > 4000 chars.
