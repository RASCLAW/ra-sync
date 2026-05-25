---
name: CC Agent Settings Read Bug
description: Command Center content gen agent only reads settings on first image generation run; mode, type, and other fields are ignored on subsequent runs
type: feedback
related: [reference_cc_command_center.md]
originSessionId: 6a7be7cb-ce51-4aec-9150-1a82c10e48aa
---
RESOLVED (2026-05-15): `/api/agent/reset` endpoint clears the AgentSession (session_id + last_ok_ts + last_error). A "Reset Agent" button in the CC triggers this before each generation run so settings are always read fresh.

**How to apply:** No longer a blocking issue. If settings bleed across runs again, check that the reset call fires before the generation prompt is sent.
