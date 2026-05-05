---
name: autoCompactWindow Setting
description: Claude Code settings.json field for autocompact behavior -- token count (100k-1M), set to 185k
type: reference
related: [feedback_settings_self_modification.md, feedback_savepoint_sweetspot.md]
originSessionId: 35152885-5b0b-4d59-98ce-4e9fe9f77f26
---
`autoCompactWindow` is the valid field in `~/.claude/settings.json` for autocompact tuning.

- **Type:** integer
- **Range:** 100,000 – 1,000,000 tokens
- **Current value:** 185,000
- **Invalid field:** `autoCompactThreshold` -- does not exist, schema rejects it

**Semantics (verified):** This is the trigger threshold -- autocompact fires when token usage hits this number. Default (no setting) = model_context_window - 45k, so ~155k on Sonnet 200k. Setting 185k pushes the fire point ~30k later, giving more usable conversation before compaction. The ~45k is hardcoded headroom reserved for the compaction summary itself.

**How to apply:** If RA wants to tune autocompact behavior, use `autoCompactWindow` with a token count. Plain `sonnet` model = 200k context; 185k leaves ~15k headroom before compaction.
