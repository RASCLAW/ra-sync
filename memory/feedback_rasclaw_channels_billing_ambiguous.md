---
name: feedback-rasclaw-channels-billing-ambiguous
description: Rasclaw runs `claude --channels` (interactive long-poll); Anthropic docs only call out `claude -p` for SDK credit pool, so --channels classification is unconfirmed
metadata:
  type: feedback
related: reference_agent_sdk_credits_june15.md, project_agent_sdk_credit_inventory.md
---

# Rasclaw --channels billing is ambiguous

Anthropic's June 15 Agent SDK credit docs explicitly list:
- `claude -p` / `claude --print` -- SDK pool
- Interactive terminal/IDE -- subscription pool

**`claude --channels plugin:telegram@...` is NOT explicitly covered.** It's a long-lived interactive session that just routes I/O via Telegram instead of stdin. Most likely interpretation: interactive bucket (subscription pool). Worst case: classified as programmatic and bills against the new credit, in which case Rasclaw becomes the single biggest burner (it's a persistent Opus 4.7 session, runs 24/7 while PC is on).

**Why:** The cost delta between the two classifications is hundreds of dollars/month. Worth resolving definitively.

**How to apply:** Before June 15, 2026, ask Anthropic support to confirm `--channels` mode classification. If SDK-billed, either (a) move Rasclaw to API-key auth (old billing, predictable), or (b) accept a much larger credit drain and downgrade Rasclaw's default model from Opus to Sonnet.

Architecture context:
- Launcher: `~/.claude/scripts/start-rasclaw.bat` → `claude --channels plugin:telegram@claude-plugins-official`
- Runs as long as RA's PC is awake and logged in
- Auto-starts via Windows Startup folder
- See [[project_rasclaw_telegram]] in ra-sync junction for full setup details
