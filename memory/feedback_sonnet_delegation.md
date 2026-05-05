---
name: Sonnet delegation policy
description: When to route work to Sonnet subagents vs. keeping it in the main Opus session. Opus stays the driver; Sonnet handles bounded grunt work.
type: feedback
related: [feedback_diagnostic_depth.md, feedback_claude_code_layers.md, feedback_closeout_format.md, feedback_batch_ingest_pattern.md, ../../../../projects/EA-brain/references/summaries/brad-claude-code-usage-limits.md]
originSessionId: d134a6f7-471e-4359-a185-57978e64b8a3
---
Opus is the driver. Sonnet is the delegate for bounded grunt work. RA doesn't switch models manually — the main Opus session calls `Agent(model: "sonnet", ...)` when a task fits the delegation profile.

**Delegate to Sonnet when ALL four are true:**
- Input spec is short (< 500 tokens to brief the subagent)
- Work is long (many tool calls, big outputs, noisy logs)
- Only a summary is needed back, not the raw work
- No mid-task decisions need RA's input

Otherwise Opus does it directly.

**Unilateral delegate (just do it, don't ask):**
- Test / regression runs (chatbot battery, smoke tests) — report pass/fail + failures
- Log scans — categorize errors, don't dump raw logs into context
- External API doc lookups — WebFetch → 5-line answer
- Scraping / data collection — raw JSON stays out of context
- Bounded audits ("which tools haven't been touched in 90 days")
- Codebase searches via Explore agent

**Ask first:**
- Bulk mechanical edits after a decision is already made (rename X → Y across N files)
- External API experiments
- Anything touching live services

**Never delegate:**
- Current conversation's in-progress work
- Decisions, memory drafting, closeout/savepoint — the "thinking" must stay in Opus because only Opus has the full conversation context
- Tasks under ~5 tool calls (briefing overhead dominates)
- Anything needing RA's input mid-task
- Closeout/savepoint/loadout — already evaluated, the mechanical work is cheap and the thinking work can't leave Opus

**RA's visibility tradeoff:**
When work is delegated, Opus only sees the subagent's summary, not the raw output. If RA needs the full output visible in the chat ("show me the logs"), don't delegate — run it directly.

**Why:** Session 102 discussion — RA asked how to integrate Sonnet into daily coding without breaking Opus context. The original backlog item "convert /closeout, /savepoint, /loadout to Sonnet" was crossed off because the thinking part (session analysis, memory drafting) can't be delegated — only Opus sees the conversation. The real win is delegating bounded, noisy, mechanical work where the spec is short and the output is summarizable.

**How to apply:**
- Default: do it directly in Opus unless all four conditions above are met
- When a task matches the "unilateral" list, call `Agent` without asking — just report the summary
- When it matches "ask first", say: "This is ~N tool calls of grunt work. Delegate to Sonnet?"
- If RA says "show me the output" or "run it directly", override delegation
