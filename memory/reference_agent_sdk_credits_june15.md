---
name: reference-agent-sdk-credits-june15
description: Anthropic Agent SDK credit pool launches June 15 2026 -- separate budget for claude -p, SDK, Code GH Actions; metered at full API rates
metadata:
  type: reference
related: project_agent_sdk_credit_inventory.md, feedback_rasclaw_channels_billing_ambiguous.md
---

# Agent SDK Credits -- effective 2026-06-15

Anthropic splits plan-based subscriptions into two billing buckets. Programmatic Claude use moves to a separate monthly credit pool, metered at **full API rates**.

## Credit by plan
- Pro: $20/mo
- Max 5x: $100/mo
- Max 20x: $200/mo
- Per-user, no rollover, drains first before usage credits

## What counts (Agent SDK pool)
- `claude -p` / `claude --print` (non-interactive Code)
- `claude_agent_sdk` Python + TypeScript SDK calls
- Claude Code GitHub Actions
- Third-party apps authenticating via Agent SDK

## What does NOT count (subscription rate-limit pool)
- Interactive Claude Code in terminal/IDE (this session)
- claude.ai web/desktop/mobile
- Claude Cowork
- `ANTHROPIC_API_KEY` auth -- bypasses entirely, stays on old API billing

## Capacity at $200 (Max 20x)
- Sonnet 4.6: ~440 medium runs / ~22M tokens
- Opus 4.7: ~265 runs / ~13M tokens
- Haiku 4.5: ~1,333 runs / ~67M tokens
- Multi-agent flows cost 1.6-2.4x more due to context summarization

## Gotcha
When credits drain, **default is hard fail with rate-limit error**. Extra usage is OFF by default -- must opt in with explicit dollar cap, or production silently breaks during an incident.

## Pre-June 15 actions
1. Inventory all `claude -p` / SDK call sites -- see [[project-agent-sdk-credit-inventory]]
2. Enable extra-usage with hard cap (manual on Anthropic billing dashboard)
3. Add per-call logging to high-volume call sites for real cost data
4. Confirm ambiguous classifications with Anthropic support -- see [[feedback-rasclaw-channels-billing-ambiguous]]

## Sources
- https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan
- https://dev.to/vainamoinen/what-anthropics-200-agent-sdk-credit-means-if-you-run-claude-p-in-production-ce2
- https://devtoolpicks.com/blog/anthropic-splits-claude-subscriptions-agent-sdk-credit-june-2026
- https://the-decoder.com/claude-subscriptions-get-separate-budgets-for-programmatic-use-billed-at-full-api-prices/
