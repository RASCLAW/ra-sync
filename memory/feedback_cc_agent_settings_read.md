---
name: CC Agent Settings Read Bug
description: Command Center content gen agent only reads settings on first image generation run; mode, type, and other fields are ignored on subsequent runs
type: feedback
related: [reference_cc_command_center.md]
originSessionId: 6a7be7cb-ce51-4aec-9150-1a82c10e48aa
---
Agent reads settings correctly on the first generation only. On subsequent runs, it ignores mode and type -- only aspect ratio, count, and product reference are applied consistently.

**Why:** Likely the session context from the first run carries forward, so the agent relies on cached instructions instead of re-reading the current form state each time.

**How to apply:** When fixing, ensure settings (mode, type, and all form fields) are re-injected into the prompt on every generation call, not assumed from session memory.
