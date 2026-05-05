---
name: Rasclaw Telegram Channel
description: LIVE -- Rasclaw is Claude Code's Telegram channel. Text + photos. Auto-starts on logon. Voice uses TG mic keyboard (not STT).
type: project
related: reference_vertex_ai.md, feedback_chatbot_architecture.md, reference_gcloud_cli.md, project_oracle_migration.md, reference_rasclaw_mobile_permissions.md, project_rasclaw_bypass_mode.md, reference_command_center.md, reference_claude_agent_sdk_install.md
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
## Status: LIVE
- Bun 1.3.11 installed, in PATH (~/.bashrc)
- Token: ~/.claude/channels/telegram/.env
- Plugin: installed via `/plugin install telegram@claude-plugins-official` in CLI
- Paired: RA's Telegram ID 1762124488, allowlist policy
- Voice: Google Speech-to-Text via ~/.claude/scripts/transcribe.py (FFmpeg .ogg -> WAV -> STT)
- Auto-start: ~/.claude/scripts/start-rasclaw.bat in Windows Startup folder
- System prompt: ~/.claude/scripts/rasclaw-system-prompt.md

## How it runs
- `start-rasclaw.bat` launches Git Bash -> `claude --channels plugin:telegram@claude-plugins-official --system-prompt-file ~/.claude/scripts/rasclaw-system-prompt.md`
- Runs as long as PC is awake and user is logged in
- Startup folder auto-launches on logon

## Key files
- `~/.claude/channels/telegram/access.json` -- allowlist + policy
- `~/.claude/channels/telegram/.env` -- TELEGRAM_BOT_TOKEN
- `~/.claude/scripts/transcribe.py` -- Google STT transcription
- `~/.claude/scripts/start-rasclaw.bat` -- launcher
- `~/.claude/scripts/rasclaw-system-prompt.md` -- voice message handling instructions

## Limitation
Claude Code must be running with --channels flag. PC sleep/shutdown kills it. 24/7 would need Oracle VM or Cloud Run.
