---
name: Savepoint Sweet Spot
description: Call /savepoint at 75% context (~150k tokens) for Sonnet 200k + 185k autoCompactWindow setup
type: feedback
related: [reference_autocompact_window.md, reference_resume_pointer.md]
originSessionId: 35152885-5b0b-4d59-98ce-4e9fe9f77f26
---
Call `/savepoint` + `/clear` when context hits **75%** (~150k tokens on Sonnet 200k).

**Why:** At 75%, savepoint adds ~8k overhead → lands at ~158k, leaving 27k margin before autocompact fires at 185k. Going past 85% risks autocompact triggering mid-savepoint. RA confirmed 75% as the target.

**How to apply:** Watch the `/context` output. At ~150k used (75%), run `/savepoint` then `/clear`. Fresh session reloads at ~37k baseline (system + memory + skills), giving ~148k of clean working context.

| Trigger point | After savepoint | Margin to 185k |
|---|---|---|
| 75% / 150k | ~158k | 27k (good) |
| 80% / 160k | ~168k | 17k (ok) |
| 85% / 170k | ~178k | 7k (risky) |
