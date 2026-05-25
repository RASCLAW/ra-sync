---
name: task-scheduler-power-policy
description: "Windows Task Scheduler with 'No Start On Batteries' + 'Stop On Battery Mode' silently skips ticks when laptop is on battery or asleep. Caused a 1h13m post delay on 2026-05-23."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - project_feed_scheduler.md
    - project_feed_scheduler_handoff_plan.md
  originSessionId: 8379745b-7f28-4b2c-aec6-f880eb146eef
---

## Rule
When using Windows Task Scheduler for time-sensitive hourly cron jobs, either flip the power policy off OR don't rely on the laptop for fire timing.

**Why:** On 2026-05-23 the hourly task `DuberyMNL_FeedScheduler` skipped the 07:00 AM tick because the laptop was on battery + sleeping. Task Scheduler reported Last Run 06:00:01, Next Run 08:00:00 -- a 2-hour gap on a "Repeat every 1 Hour(s)" task. Result: a post scheduled for 06:45 fired at 08:02, 1h13m late. The default policy `No Start On Batteries` + `Stop On Battery Mode` silently no-ops the tick instead of catching up.

**How to apply:**
- Quick fix: Task Scheduler -> task properties -> Conditions tab -> uncheck both `Start the task only if the computer is on AC power` and `Stop if the computer switches to battery power`. Solves unplugged-but-awake case. Does NOT solve sleep/off.
- Real fix: don't rely on a laptop for time-sensitive cron. For DuberyMNL feed posts, hand off to Meta via `scheduled_publish_time` so Meta's servers fire the post (see [[project_feed_scheduler_handoff_plan]]).
- Diagnostic: when a scheduled job runs late or not at all, check Task Scheduler's `Last Run Time` vs `Next Run Time` for gaps. A 2-hour gap between consecutive hourly runs is a smoking gun for the battery policy.

## Related
- [[project_feed_scheduler]] -- the v1 system this gotcha surfaced in
- [[project_feed_scheduler_handoff_plan]] -- the real fix (Meta-native scheduling)
