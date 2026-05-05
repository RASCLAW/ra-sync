---
name: Claude Code 4-Layer Architecture
description: L1 CLAUDE.md, L2 Skills, L3 Hooks (missing), L4 Agents -- hooks are the gap to fill
type: feedback
related: [project_llm_wiki_system.md, feedback_sonnet_delegation.md, ../../../../projects/EA-brain/references/summaries/brad-claude-code-usage-limits.md, reference_build_flow.md]
originSessionId: d134a6f7-471e-4359-a185-57978e64b8a3
---
Claude Code has a 4-layer architecture (from Brij Kishore Pandey's cheatsheet, session 95):

- **L1 -- CLAUDE.md:** Persistent context and rules. RA has this solid (global + project).
- **L2 -- Skills:** Auto-invoked knowledge packs. RA has 20+ production skills.
- **L3 -- Hooks:** Deterministic callbacks (PreToolUse, PostToolUse, Notification). **RA has NOT set these up yet.**
- **L4 -- Agents:** Subagents with their own context. RA has dubery-content, dubery-ads, dashboard.

**Hooks (L3) -- the gap:**
- PreToolUse: runs before a tool executes. Exit 0 = allow, exit 2 = block.
- PostToolUse: runs after a tool completes. Auto-lint, auto-format.
- Notification: fires on Claude notifications. Could route to Telegram/Rasclaw.
- Configured in `settings.json` under `"hooks"` key.
- Can match specific tools (Bash, Edit, Write) or `*` for all.

**Permissions complement hooks:**
- `allow`/`deny` patterns in settings.json = simple gates (no logic)
- Hooks = programmable gates (run a script, inspect input, decide)

**Why:** Hooks automate safety checks RA currently does manually (no force-push, no .env writes, etc.). Notification hook could alert Rasclaw when long tasks finish.

**How to apply:** Set up hooks when RA is ready. Start with PreToolUse to block destructive Bash commands, then add Notification hook for Telegram integration.

**Context optimization (from Brad's video, applied session 114):**
- `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=75` env var in settings.json -- compact at 75% instead of default ~83%
- `BASH_MAX_OUTPUT_LENGTH=150000` env var -- prevent silent truncation + expensive retries
- Progressive disclosure: decisions/log.md + me.md + work.md + facts.md moved from @-includes to on-demand reads (~55KB/message savings)
- 5 parked v1 skills archived to `.claude/skills-archive-v1/` (dubery-ad-creative, dubery-prompt-writer, dubery-prompt-validator, dubery-infographic-ad, dubery-ugc-fidelity-gatekeeper)
- Deny rules for irrelevant directories still TODO
See [Brad -- Claude Code Usage Limits](../../../../projects/EA-brain/references/summaries/brad-claude-code-usage-limits.md).
