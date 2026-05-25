---
name: meta-graph-api-subcode-99
description: "Meta Graph API http 500 + error_subcode 99 + 'An unknown error occurred' is a generic transient error. Retry usually works. Not a bug in our code."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - project_feed_scheduler.md
    - project_feed_scheduler_handoff_plan.md
  originSessionId: 8379745b-7f28-4b2c-aec6-f880eb146eef
---

## Rule
When Meta Graph API returns `{"error":{"code":1,"message":"An unknown error occurred","error_subcode":99}}` with HTTP 500, treat it as a transient Meta-side glitch. Retry usually works on the next attempt.

**Why:** On 2026-05-23 the Outback Green feed post (`feed-20260523-0645-001`) failed at 08:02 with this exact error after the Outback Blue post submitted seconds earlier (and through the same token, same code path) succeeded. Same code, same token, same payload shape, same minute -- only the image differed. Classic transient.

**How to apply:**
- Don't dig into code or payloads when you see subcode 99 in isolation. Retry first.
- If you see subcode 99 across 3+ sequential retries with the same payload, THEN start looking at the specific image/caption for something unusual (corrupt PNG, oversized file, unicode quirk in caption, etc).
- For the feed scheduler handoff path: gate retries with a `handoff_attempts < 3` counter (already designed in [[project_feed_scheduler_handoff_plan]]).
- Manual recovery for failed queue items: flip status from `FAILED` back to `APPROVED` in `feed_queue.json` and re-run `python tools/facebook/post_from_queue.py`.

## Related
- [[project_feed_scheduler]] -- the system where this surfaces
- [[project_feed_scheduler_handoff_plan]] -- the retry-safety-net design that absorbs subcode 99s
