---
name: schedule-chat-vs-agent-chat
description: "Two separate CC chat endpoints with different system prompts, sessions, and context loads. /api/agent/chat = Content Gen (shared session, full project skills). /api/schedule/chat = Schedule AI Suggest (per-tab session, narrow brand prompt + holidays)."
metadata: 
  node_type: memory
  type: reference
  related: 
    - project_schedule_v2_shipped.md
    - reference_command_center.md
    - reference_agent_sdk_credits_june15.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

| Endpoint | Used by | Session model | System prompt | Tools |
|---|---|---|---|---|
| `/api/agent/chat` (SSE stream) | Content Gen Direction chat, floating bot FAB | Single `AgentSession` singleton, shared across all users of CC | Implicit — `settingSources=['project']` loads CLAUDE.md + every DuberyMNL skill | All Claude Code tools (bypassPermissions) |
| `/api/schedule/chat` (POST JSON) | Schedule AI Suggest tab | Per-tab UUID in localStorage, in-memory dict server-side `_SCHED_CHAT_SESSIONS` | Explicit — baked brand facts + caption/time format + auto-injected upcoming-holidays block | `Read` only (`allowed_tools=['Read']`) |

## Why two
- Content Gen needs deep project context (skills, brand specs, prompt JSONs) so settingSources='project' makes sense — it loads ALL skills/CLAUDE.md
- Schedule AI Suggest is a focused caption brainstorm — doesn't need the full skill bundle, lighter system prompt = faster + cheaper per turn
- Different session models = isolated state (Content Gen agent doesn't pollute Schedule chat history)

## Both go through `claude_agent_sdk.query()`
Same SDK, same Pro-plan auth path today. Both route to the new Agent SDK metered credit pool after 2026-06-15 (see [[reference_agent_sdk_credits_june15]]).

## Auto-injected holidays
- Schedule chat: bakes upcoming-holidays into the SYSTEM prompt at request time via `_build_sched_chat_system_prompt()` — always there
- Content Gen Direction: prepends holidays as a USER-side context block in `askDirection()` via cached `/api/schedule/upcoming-holidays?days=14` — only on Direction submits, not the floating bot FAB

## When to extend which
- Adding a brainstorm/copywriting feature → mirror the Schedule chat pattern (per-session, narrow system prompt, allow-list tools)
- Adding an orchestrator that needs to drive multiple skills → use the shared `AgentSession` pattern with settingSources='project'

## Related
- [[project_schedule_v2_shipped]] — where the Schedule chat lives
- [[reference_command_center]] — general CC architecture
- [[reference_agent_sdk_credits_june15]] — billing change that affects both
