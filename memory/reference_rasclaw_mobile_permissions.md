---
name: Rasclaw Mobile Permission Sandbox
description: Pre-approved permission set in ~/.claude/settings.json that lets the Rasclaw Telegram bot run research + save workflows without prompting RA. Rasclaw-scoped writes only.
type: reference
related: project_rasclaw_telegram.md, reference_telegram_send_patterns.md, feedback_claude_code_permissions.md
originSessionId: eba69f11-3dc7-4a1f-a896-98c4118a1506
---
The mobile Telegram workflow (RA messages bot -> Claude acts) needs broad pre-approval because RA can't see/tap permission prompts from his phone. The sandbox is enforced by Write/Edit path globs scoped to Rasclaw.

## What's pre-approved (as of session 99)

**Telegram plugin I/O:**
- `Read(**/channels/telegram/inbox/**)` -- read any photo/voice the plugin drops in
- `Bash(cp *)` -- copy from telegram inbox to project folders
- `mcp__plugin_telegram_telegram__reply` -- send text/photo replies
- `mcp__plugin_telegram_telegram__react` -- emoji reactions

**Research tools (any domain):**
- `WebFetch` -- review any URL
- `Bash(gh *)` -- full gh CLI (repo view/clone, search, pr diff, etc.)
- `Bash(curl *)` -- HTTP fetches, downloads
- `Bash(python *)` + `Bash(git *)` -- already global, covers /youtube skill + clones

**Rasclaw project writes:**
- `Read(**/Rasclaw/**)`, `Write(**/Rasclaw/**)`, `Edit(**/Rasclaw/**)`
- `C:\Users\RAS\projects\Rasclaw` added to `additionalDirectories`

## What still prompts (by design)
- Writes anywhere OUTSIDE Rasclaw (no global Write)
- `rm -rf`, `git push --force`, `git reset --hard`, `.env` reads -- deny list
- Voice ffmpeg transcode (`.oga -> .wav`) -- not yet pre-approved, images only

## Typical flows that run prompt-free
- "Review github.com/X/Y and save notes to Rasclaw" -- gh clone + Read + Write
- "Review this URL: ..." -- WebFetch + reply
- "Summarize this YouTube video: ..." -- python /youtube skill + reply
- "Save this photo as [description]" -- cp from telegram inbox to `~/projects/Rasclaw/inbox/images/`

## Gotcha
Settings are loaded at session start. The telegram channel is a SEPARATE Claude Code process from the VSCode session -- edits from VSCode don't reach it until `~/.claude/scripts/start-rasclaw.bat` relaunches the telegram session.
