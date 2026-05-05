---
name: Claude Agent SDK Install + Subscription Auth
description: pip install claude-agent-sdk works on Windows. Inherits Claude Code subscription auth (no API key needed). ~$0.24 first-call cache-create, cheap resume.
type: reference
related: reference_command_center.md, project_command_center_phase1_complete.md, project_rasclaw_telegram.md, reference_claude_cli.md
originSessionId: 65f454a5-f69c-4ea7-9925-80aa4bafc8cc
---
## What it is
Python package that wraps the Claude Code CLI programmatically. `query(prompt, options)` spawns a persistent Claude session under the hood -- same thing Rasclaw does via the Telegram plugin, but callable from any Python app.

## Install
```bash
pip install claude-agent-sdk
```
Pulls ~15 deps (mcp, httpx-sse, starlette, uvicorn, pydantic-settings, jsonschema, pywin32).
Verified working on Windows 11 via VSCode tunnel from work laptop (2026-04-18, session 130).

## Auth
Inherits Claude Code subscription auth automatically. No `ANTHROPIC_API_KEY` needed if `claude login` has been run and Claude CLI is authenticated on the same machine. Falls back to API key when env var is set (useful for production/headless deploys, but pay-per-token).

Smoke test confirms session + tools + project context all work:
```python
from claude_agent_sdk import query, ClaudeAgentOptions
options = ClaudeAgentOptions(max_turns=1, system_prompt="Reply exactly SDK_OK")
async for msg in query(prompt="go", options=options):
    print(type(msg).__name__, msg)
```
Returns SystemMessage(init) + AssistantMessage(TextBlock("SDK_OK")) + RateLimitEvent + ResultMessage(success, $0.24).

## Cost model
- **First call in a session: ~$0.24** -- cache_creation_input_tokens loads full project CLAUDE.md + all skills (~38k tokens).
- **Subsequent calls with `resume=session_id`: cheap** -- cached tokens read for far less. Use one AgentSession module-level singleton and reuse its session_id.
- Five-hour rate-limit window on subscription; SDK returns RateLimitEvent with `resets_at` so backend can react.

## Key options for command-center pattern
```python
ClaudeAgentOptions(
    cwd=str(PROJECT_ROOT.as_posix()),          # POSIX string avoids Windows backslash quirks
    setting_sources=["project"],                # auto-loads .claude/skills/ + CLAUDE.md
    max_turns=10,
    permission_mode="bypassPermissions",        # fine for single-user local; swap for allowlist before multi-tenant
    resume=self.session_id,                     # None on first call, session_id after
)
```

## Remote install caveat
RA successfully installed + tested via VSCode tunnel from work laptop -- no need to be physically at home. Auth carries over because Claude CLI is already logged in locally.

## When to pick SDK vs alternatives
- **Agent SDK (this)** -- want programmatic streaming + session reuse + skills inheritance -> default pick
- **`claude --print` subprocess** -- one-shot CLI call, no session memory, simpler but slower, useful for cron
- **Claude Code channel plugin** (like telegram plugin Rasclaw uses) -- full CLI instance with a channel-specific event loop, needs writing a plugin if channel doesn't exist (web channel doesn't exist as of 2026-04)
- **Anthropic SDK (anthropic package)** -- pay-per-token API, no skills/CLAUDE.md inheritance unless you hand-feed them

## Breaking change watch
Package is pre-1.0 (0.1.61 as of install). API may shift; pin version in requirements.txt.
