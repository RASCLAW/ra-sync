---
name: DuberyMNL Command Center
description: Local web dashboard + persistent Claude Agent SDK backend. Phase 1 MVP live on localhost:8090. Portfolio + personal ops + future client template.
type: reference
related: project_command_center_phase1_complete.md, reference_claude_agent_sdk_install.md, project_rasclaw_telegram.md, feedback_register_rebinds_bug.md, project_command_center_phase2_scoping.md, feedback_form_always_randomizes.md
originSessionId: 65f454a5-f69c-4ea7-9925-80aa4bafc8cc
---
## What it is
Local web dashboard for DuberyMNL ops with a persistent Claude Agent SDK backend session. Built session 130 (2026-04-18) as Phase 1 MVP -- portfolio-facing + personal ops + future RAS Creative client template.

## Where it lives
`c:/Users/RAS/projects/DuberyMNL/command-center/` -- inside the DuberyMNL repo so it reuses `.env`, `tools/`, and `.claude/skills/` directly.

## How to run
```
python command-center/app.py
```
Serves `http://localhost:8090`. UTF-8 console, CORS for localhost, request logging to stdout.

Task Scheduler entry (Phase 2 work): boot.bat registered as `DuberyMNL-CommandCenter` at-logon, same pattern as `DuberyMNL-Chatbot` + `DuberyMNL-Tunnel`.

## Port map (DuberyMNL services)
- 8080: chatbot (Flask)
- 8090: command center (Flask) <-- this
- 8123: review.duberymnl.com tunnel target
- 8124: tag.duberymnl.com tunnel target

## Architecture
```
Browser  <->  Flask (app.py, :8090)  <->  AgentSession  <->  Claude Agent SDK  <->  Claude
                    |                                                 |
                    +-- /api/home/summary                              |
                    +-- /api/monitor/status (9 monitors, parallel)    |
                    +-- /api/monitor/logs/<service>                    |
                    +-- /api/agent/chat (SSE)  -----------------------+
                                                setting_sources=['project'] so all
                                                DuberyMNL skills + CLAUDE.md load
```

Vanilla HTML+CSS+JS frontend (no React/Next). Hash-based tab routing with custom `tab:activated` event. Light Claude AI theme `#f7f5f0` + warm accent `#e07a3a`, Inter font. (Dark theme was Phase 1, switched to light session 133.)

## Phase 1 scope (DONE, 46 tasks)
- Backend skeleton + SSE chat endpoint + monitor endpoints + home summary
- 9 monitor modules: chatbot, tunnel, worker_fallback, meta_ads, story_rotation, rasclaw_tg, chatbot_tg, crm_sheet, inventory
- Shell: sidebar + 8 tabs + dark theme
- Home tab (4 tiles + system health pill)
- Monitor tab (Option B dense rows, auto-poll 30s cheap, manual expensive, modal logs, relative timestamps)
- Floating Claude chat FAB (click-to-open, SSE stream, localStorage history)

## Phase 2 scope (PARTIAL -- Content Gen done session 133)
- Content Gen tab: DONE. Mode (UGC/Brand/Bespoke) + Type + Count + Product picker + Direction mini-chat (image paste + conversational confirm) + SSE streaming output + validation checklist + image result cards + lightbox + server-side history + generation logging + toast notifications + monitor fix buttons
- Marketing tab: NOT BUILT. Caption gen, creative picker, "Run Ads" actions
- Proactive bot bubbles: NOT BUILT. Event-driven + periodic safety net (hybrid trigger)

## Phase 3 scope (not built)
- CRM tab (orders pipeline from Sheet + source attribution)
- Chatbot tab (live convos feed + handoff queue + /mark-sale captures)
- Image Bank tab (chatbot bank + FB stories pool + hero shots + prodref library)
- Inventory tab (per-SKU counts + low-stock alerts)
- Polish + demo video + Task Scheduler autostart

## Key design picks
- Single persistent AgentSession (session_id reused across requests) -- first call pays ~$0.24 cache-create, subsequent calls cheap
- Monitor layout: Option B (dense vertical rows), picked via /brainstorm vs card grid / status board
- Proactive trigger: hybrid (event-driven + periodic)
- `cwd` passed as `Path.as_posix()` string to avoid Windows backslash quirks in `setting_sources=['project']`
- `permission_mode=bypassPermissions` for single-user local tool -- must swap to allowlist before any multi-tenant exposure

## Not wired in Phase 1
- `.env` additions still pending: `WORKER_URL`, `GITHUB_TOKEN`, `RASCLAW_BOT_TOKEN`. Monitors degrade to `not_wired` gracefully when absent.
- Task Scheduler autostart (boot.bat exists, XML stub in README)

## Files
- `app.py` -- Flask entry, routes, SSE glue, image serving, upload, stats, history, fix endpoints
- `agent_session.py` -- AgentSession singleton + session reuse (max_turns=30)
- `monitors/__init__.py` -- ServiceStatus dataclass + register()
- `monitors/registry.py` -- wires all 9 monitors
- `monitors/*.py` -- one module per service
- `templates/shell.html` + `templates/tabs/*.html`
- `static/css/main.css` + `static/js/{shell,home,monitor,bot,content_gen,toast}.js`
- `boot.bat`, `.env.example`, `README.md`, `requirements.txt`

## Full plan
`.tmp/plan.md` (may be overwritten by future /plan runs) -- commit preserves session 130 state.
