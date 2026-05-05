---
name: Rasclaw Bypass Mode + Guard
description: Rasclaw runs in --permission-mode bypassPermissions, isolated via RASCLAW_MODE=1 env var, with PreToolUse guard blocking destructive ops. System prompt narrates progress to fight phone silence.
type: project
related: [project_rasclaw_telegram.md, reference_rasclaw_mobile_permissions.md, feedback_claude_code_permissions.md, feedback_claude_code_layers.md]
originSessionId: rasclaw-bypass-mode
---

Rasclaw operates in Claude Code's `bypassPermissions` mode so phone-based requests don't trigger approval fatigue. Safety comes from a hook-level deny list, not the normal allowlist.

**Why:** Previous behavior hit ~20+ permission prompts for a simple "pull 3 images from the bank" request — settings.json allowlist didn't cover `Read(**)` or `mcp__plugin_telegram_telegram__send_photo`. Curated allowlist kept growing and still had gaps. Session 127 (2026-04-16) rebuilt the model around a safety-net hook instead.

**How to apply:** When editing Rasclaw behavior, touch these five files together — they're a system. Don't add permissive rules to settings.json for Rasclaw's benefit; add safety rules to the guard instead.

## The five files

1. **`~/.claude/scripts/start-rasclaw.bat`** — launcher
   - Sets `RASCLAW_MODE=1` (cmd + bash, so env var survives the shell hop)
   - `cd ~/projects/DuberyMNL` so CLAUDE.md auto-loads
   - `claude --permission-mode bypassPermissions --channels plugin:telegram@... --system-prompt-file ...`

2. **`~/.claude/scripts/rasclaw-guard.py`** — PreToolUse hook
   - Reads hook JSON from stdin
   - Checks `RASCLAW_MODE` env var; exits 0 immediately if not set (local Claude Code untouched)
   - For Bash: regex-matches `tool_input.command` against destructive patterns (rm -r, git push, reset --hard, rebase, mv, shutdown, destructive SQL, gh destructive, vercel rm)
   - For Write/Edit/NotebookEdit: matches `file_path` against .env/credentials/secrets/token
   - Exit 2 with stderr = block (Claude sees the reason)

3. **`~/.claude/scripts/rasclaw-system-prompt.md`** — personality + responsiveness
   - Operating Mode section tells Claude it's in bypass mode + how to react to guard blocks (tell RA, don't work around)
   - Responsiveness rules: ack immediately, narrate plans for 3+ tool calls, progress pings for >15s ops, keep replies short
   - Image requests section lists bank paths inline for fast Glob routing

4. **`~/.claude/settings.json`** — hook registration
   - `hooks.PreToolUse` matcher: `"Bash|Write|Edit|NotebookEdit"`
   - Command: `python ~/.claude/scripts/rasclaw-guard.py`
   - The existing Notification hook (message box on attention) stays unchanged

5. **`~/projects/DuberyMNL/CLAUDE.md`** — directory map
   - File Rules section expanded with `contents/ready/` + `contents/assets/` subtrees
   - Auto-loaded because launcher cd's into repo. Rasclaw inherits bank paths without duplication in system prompt.

## Isolation model

`RASCLAW_MODE=1` is the gate. Set only in the launcher env. Local Claude Code sessions never set it, so guard exits 0 immediately — zero perf cost, zero behavior change for desktop work.

## Extending the guard

To block a new destructive pattern: edit `BASH_DENY` list in guard.py. Tuple format: `(regex, human_reason)`. Test via:
```bash
echo '{"tool_name":"Bash","tool_input":{"command":"YOUR_CMD"}}' | RASCLAW_MODE=1 python ~/.claude/scripts/rasclaw-guard.py
```
Exit 0 = allowed, exit 2 = blocked with reason printed to stderr.

## Known tradeoffs

- `mv` / rename blocked — strict default. Relax if it breaks legitimate bank-curation flows.
- `git push` blocked entirely. All pushes stay on PC sessions.
- Guard is Python. Requires python in PATH (confirmed present in RA's env per settings.json patterns).
