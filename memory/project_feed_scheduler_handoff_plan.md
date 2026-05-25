---
name: feed-scheduler-handoff-plan
description: 2026-05-23 session 172 -- 14-task plan for Meta-native scheduled_publish_time handoff. NOT executed yet. .tmp/plan.md is the source of truth.
metadata: 
  node_type: memory
  type: project
  related: 
    - project_feed_scheduler.md
    - feedback_task_scheduler_power_policy.md
    - feedback_meta_subcode_99_transient.md
    - feedback_wf3a_unblocked.md
  originSessionId: 8379745b-7f28-4b2c-aec6-f880eb146eef
---

## Status
**PLAN DRAFTED 2026-05-23 (session 172). NOT executed.**
Source of truth: `.tmp/plan.md` in DuberyMNL repo root (gitignored, lives only locally).

## Why
Scheduled post fired 1h13m late because Windows Task Scheduler skipped the 07:00 AM cron tick (laptop unplugged + asleep -- see [[feedback_task_scheduler_power_policy]]). Even with the power-policy toggle fix, the multi-day-offline case still drops posts entirely. Need Meta to be the one firing the post, not our laptop.

## Design (one-liner)
At queue time, hand the post off to Meta via Graph API `scheduled_publish_time`. Meta fires it from their servers. Our cron becomes a retry safety net (for failed handoffs) + a verify pass (flip `SCHEDULED_AT_META` -> `POSTED` after Meta fires).

## New status state machine
```
APPROVED  --handoff ok-->  SCHEDULED_AT_META  --verify-->  POSTED
   |                              |
   |                              +--cancel-->  CANCELLED (FB deleted)
   |
   +--cron fire (<10min lead or 3 handoff fails)-->  local publish --> POSTED / FAILED
```

## 14 tasks (summary)
1. Verify Graph API contracts (research, no code)
2. Create `tools/facebook/scheduled_handoff.py` skeleton
3. `eligible_for_handoff(item, now)`
4. `handoff_to_meta()` single + collage path
5. `handoff_to_meta()` multi-photo path
6. `cancel_at_meta()` + `verify_published()`
7. Wire handoff into `/api/schedule/add`
8. Wire cancel into `/api/schedule/cancel`
9. Worker handoff-retry + verify passes in `post_from_queue.py`
10. Extend dry-run output for 3 groups (HANDOFF / VERIFY / DUE_LOCAL)
11. Frontend: blue ON META pill + cancel/submit UX
11.5. Backend: include `SCHEDULED_AT_META` in upcoming filter
12. CSS for blue pill
13. CLI parity in `queue_add.py`
14. End-to-end live test (close-lid scenario is the proof)

## Per-item field additions
- `fb_scheduled_post_id` (string|null)
- `handoff_attempted_at` (iso|null)
- `handoff_error` (string|null)
- `handoff_attempts` (int, default 0)

## Risks called out
- Meta subcode 99 transients (see [[feedback_meta_subcode_99_transient]]) -- mitigated by `handoff_attempts < 3` retry counter
- Timezone conversion to unix UTC -- `int(parse_iso(s).timestamp())` with tz-aware dt
- Multi-photo quirk: some Graph API versions need `scheduled_publish_time` on each photo upload, not just feed POST (Task 1 verifies)
- Double-fire if app.py response dropped between handoff success and status flip -- mitigate with verify-first on retry
- Cancel race at fire boundary -- verify after DELETE; if `is_published`, mark POSTED instead of CANCELLED

## Estimate
2.5-3 hours coding + ~30 min wall time for live test.

## Files touched
- `tools/facebook/scheduled_handoff.py` (NEW)
- `tools/facebook/post_from_queue.py` (MODIFIED)
- `tools/facebook/queue_add.py` (MODIFIED, small)
- `command-center/app.py` (MODIFIED -- 2 routes + upcoming filter)
- `command-center/static/js/schedule.js` (MODIFIED -- pill + cancel UX)
- `command-center/templates/tabs/schedule.html` (MODIFIED -- CSS rule for blue pill)

## Related
- [[project_feed_scheduler]] -- the v1 SHIPPED scheduler this builds on
- [[feedback_task_scheduler_power_policy]] -- the gotcha that motivated this plan
- [[feedback_meta_subcode_99_transient]] -- the Graph API quirk we have to handle
- [[feedback_wf3a_unblocked]] -- prerequisite (PAGE_ACCESS_TOKEN has pages_manage_posts)
