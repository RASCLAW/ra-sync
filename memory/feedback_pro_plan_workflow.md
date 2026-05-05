---
name: Pro Plan Workflow (budget-constrained mode)
description: Behavioral rules and billing facts for operating Claude Code on the $20 Pro plan — model choices, subagent restraint, /loop ban, API key safety
type: feedback
related: [feedback_sonnet_delegation.md, reference_claude_cli.md, feedback_claude_code_layers.md]
originSessionId: 545c83ca-7691-44bd-b4f0-4e1f98f8b71e
---
When RA is on Claude Pro ($20), not Max 5x, operate under these constraints. Triggered by RA saying he's on Pro, downgraded, or budget-constrained.

**Why:** Max 5x ($100) → Pro ($20) means real capability loss, not just slower. Claims I made in session 2026-04-15 about Pro were partially wrong and got corrected by verification. Budget matters more than speed right now — wrong billing assumptions cost real money.

**How to apply:** Load these rules whenever RA confirms Pro tier. Re-verify when he upgrades back to Max.

## Billing facts (verified 2026-04-15)

- **Pro has NO Opus access.** Sonnet 4.6 + Haiku 4.5 only. Opus requires Max plan. Do not suggest Opus for planning/debugging on Pro.
- **Pro quota is message-based, not token-based.** All models share the ~45 messages per 5-hour rolling window. Weekly cap also applies (added Aug 2025).
- **Haiku does not extend Pro quota.** Same message count as Sonnet. Use Haiku only for genuinely trivial tasks where quality difference doesn't matter.
- **`/fast` does NOT save Pro quota.** It bills to extra usage at higher per-token cost. Don't recommend it as a Pro-budget strategy.
- **`ANTHROPIC_API_KEY` overrides subscription, not falls back.** If set in env, Claude Code bills API rates even when Pro quota is available. Never leave it live alongside Pro without RA knowing.
- **Subagents count against parent's 5-hour quota.** Parallel Explore/general-purpose spawns drain faster. Not free.

## Session discipline rules

- **Single active session.** No multi-window work on Pro. Close VSCode tabs cleanly, kill orphans via `pc-status`.
- **One goal per session.** State it in the first message. `/closeout` when done — don't leave idling sessions open.
- **Prefer inline Grep/Glob over Explore agent** unless the search genuinely needs 3+ query rounds.
- **No `/loop`.** Re-reads context per iteration, burns quota fast.
- **No parallel subagent spawns.** One at a time if truly needed.
- **Read only files needed for the edit.** Skip "just getting context" reads.
- **No `/ingest` batches.** One source per session.

## Tools that do NOT touch Pro quota (safe to use heavily)

- `generate_vertex.py` — Gemini/Vertex billing, separate from Claude
- `dubery-landing/` static edits
- Chatbot runtime (own API keys)
- Any pure-Python tool without `anthropic` import

## Tools that call Anthropic API directly (cost real money on API key, not Pro quota)

- `tools/upwork/scout.py`
- `tools/upwork/market_intel.py`
- Chatbot conversation engine (if Anthropic-backed — verify per deployment)

## Per-task playbook

| Task | Approach |
|------|----------|
| Caption gen | Sonnet, skill only, no subagents |
| Image prompt | `/dubery-fidelity-prompt` → `/dubery-prompt-reviewer`, both Sonnet |
| Chatbot tweak | Read → Edit → test, no /plan for <3 file changes |
| Cloud Run migration | Split 16-task plan across 3-4 sessions |
| Ad analysis | Inline reads, skip `dubery-ads` subagent unless pulling Meta data |
| Ingest | Single source, manual skill run |

## Escape hatches

- Hit 5-hour Pro cap mid-session → pause or switch to API billing (set key, finish, unset)
- Hit weekly cap → API pay-as-you-go for rest of week
- Genuine Opus need → temporary API key with explicit budget cap, RA-confirmed

## Monthly budget frame

- Pro: $20 flat
- API spillover (realistic heavy week): $15-30
- Worst case: $35-50/mo vs Max 5x's $100
- Savings: $50-65/mo → ads, groceries, or job-hunt reserves

## Sources

Verified via claude-code-guide agent against: Claude Support Pro limits docs, code.claude.com/docs, truefoundry.com Claude Code limits guide, GitHub issue #2944 (API fallback feature request). Any doc drift after 2026-04-15 should trigger re-verification.
