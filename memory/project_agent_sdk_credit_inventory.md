---
name: project-agent-sdk-credit-inventory
description: Inventory of which RA systems will / will not bill against Agent SDK credit pool after June 15 2026 cutover
metadata:
  type: project
related: reference_agent_sdk_credits_june15.md, feedback_rasclaw_channels_billing_ambiguous.md
---

# Agent SDK Credit Cutover -- Per-System Inventory (2026-05-20)

See [[reference-agent-sdk-credits-june15]] for the policy itself.

## Affected (draws from credit pool)

| Path | Mechanism | Model | Notes |
|---|---|---|---|
| `DuberyMNL/command-center/agent_session.py` | `claude_agent_sdk` Python SDK | session default | Powers CC Agent content gen. Singleton w/ session resume helps via cache. |
| `Knowledgebase-informdata/conversation_engine.py:101` | `claude --print` subprocess | Haiku | Work KB chatbot. Cheap. |
| `ra-dashboard/tools/telegram/arabelle_bot.py:240` | `claude --print` subprocess | **Sonnet** (was Opus, swapped 2026-05-20) | Arabelle TG bot. Opus→Sonnet = ~40% savings. |
| ra-dashboard moderator cron | `claude --print` via local cron, 15-min interval | Sonnet | Dashboard sync (per `decisions/log.md` 2026-03-25). |
| Backlog: moderator as Claude Routine | Future Routines | TBD | **Hold until after 2026-06-15** so it gets re-costed at real pricing. |

## NOT affected

| System | Why |
|---|---|
| **DuberyMNL Messenger chatbot** (`chatbot/`) | Google Vertex AI, not Claude |
| **CF Worker FAQ layer** | Worker JS, no Claude |
| **Rasclaw TG bot** (`Rasclaw/scripts/start-rasclaw.bat`) | `claude --channels` (interactive long-poll, NOT `-p`). Likely subscription pool -- see [[feedback-rasclaw-channels-billing-ambiguous]] |
| **DuberyMNL `tools/chatbot/`** | Legacy stale per CLAUDE.md, not running |
| **GH Actions `story-rotation.yml`** | Pure Python + Meta API |
| **`tools/upwork/scout.py` + `market_intel.py`** | Local keyword scan, no Claude |
| **Interactive Claude Code sessions** | Subscription pool |

## Estimated monthly burn at $200 (Max 20x)
- Arabelle bot (Sonnet, ~150 runs/mo): ~$67
- CC agent gen: ~$20-50
- Knowledgebase-informdata (Haiku): <$5
- Moderator cron: $10-30
- **Total: ~$100-150/mo** -- comfortable margin after Sonnet swap.

## Open items
1. Confirm Rasclaw `--channels` billing with Anthropic
2. Enable extra-usage hard cap on Anthropic billing dashboard
3. Restart Arabelle bot to pick up Sonnet model
4. Add per-call cost logging to `agent_session.py` + `arabelle_bot.py` before June 15 for real-spend data
