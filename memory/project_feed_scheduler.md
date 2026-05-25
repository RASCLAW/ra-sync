---
name: feed-post-scheduler
description: "2026-05-21 session 166 -- SHIPPED. Queue-based FB feed scheduler (multi-image + collage) + CC Schedule tab + hourly Windows cron. 4 live posts validated. Pattern B (local queue, not Meta native)."
metadata:
  node_type: memory
  type: project
  related:
    - project_command_center_phase1_complete.md
    - reference_cc_command_center.md
    - feedback_wf3a_unblocked.md
    - project_schedule_tab_v1.md
    - project_schedule_v2_shipped.md
    - project_cc_sidebar_collapse.md
    - project_schedule_tab_v2_plan.md
    - project_feed_scheduler_handoff_plan.md
    - feedback_task_scheduler_power_policy.md
    - feedback_meta_subcode_99_transient.md
  originSessionId: eca7da81-9a6e-484a-960a-59e08728160d
---

## Status
**SHIPPED 2026-05-21 (session 166).** Cron registered, 4 live posts validated, CC tab live at `https://cc.duberymnl.com/#schedule`.

## Architecture (final)
- Queue: `tools/facebook/feed_queue.json` (gitignored)
- Worker: `tools/facebook/post_from_queue.py` runs hourly via Windows Task `DuberyMNL_FeedScheduler`
- Worker routes by mode: `multi` → single or multi-photo Graph API call; `collage` → Pillow composes via `tools/image_ops/compose.py`, posts as one image
- TG ping on success + failure
- 7 collage layouts: `2h`, `2v`, `1p2`, `2x2`, `3h`, `hero3`, `ba` (all 1080x1080 PNG, 2px gutters)

## Files shipped
- NEW: `tools/facebook/queue_helpers.py`, `queue_add.py`, `post_from_queue.py`
- NEW: `tools/image_ops/compose.py`
- NEW: `command-center/templates/tabs/schedule.html`, `command-center/static/js/schedule.js`
- MODIFIED: `command-center/app.py` (5 routes), `templates/shell.html`, `static/js/shell.js`, `static/css/main.css`, `.gitignore`

## Cron registration command
```
schtasks /Create /SC HOURLY /MO 1 /TN "DuberyMNL_FeedScheduler" \
  /TR "C:\Users\RAS\AppData\Local\Programs\Python\Python312\python.exe C:\Users\RAS\projects\DuberyMNL\tools\facebook\post_from_queue.py" \
  /ST 23:00 /F
```
Absolute Python path required (HKCU PATH wiped, default `python` not found).

## Live validation (4 posts)
- `111349974035733_1424054179748534` -- single-photo
- `111349974035733_1424058753081410` -- multi-photo (2 images)
- `111349974035733_1424065623080723` -- collage layout `2h` (composed → single photo)
- `111349974035733_1424104896410129` -- cron-fired single, proved unattended hourly works

## Gotchas captured during build
- Default `python` not in PATH (HKCU wipe) -- always use absolute path for cron task `/TR`
- File-not-found errors were mislabeled `network:` -- added explicit `Path.exists()` check before `requests.post`
- File-lock fcntl/msvcrt pattern works on Windows; queue files atomic-written via `os.replace`
- Laptop sleep skips cron -- 2026-05-23 incident proved the multi-day-offline case is real (1h13m late post). Hybrid Meta-native handoff PLANNED, see [[project_feed_scheduler_handoff_plan]]
- Task Scheduler default battery policy silently no-ops ticks when on battery/asleep -- see [[feedback_task_scheduler_power_policy]]
- Meta Graph API subcode 99 transients hit this path -- see [[feedback_meta_subcode_99_transient]]

## v2 plan ready (NOT yet built)
See [[project_schedule_tab_v2_plan]] -- AI Suggest chat + Calendar in 3 top tabs. RA chose to use v1 in real workflow first.

## Related
- [[project_schedule_tab_v1]] -- v1 capability catalog
- [[project_schedule_tab_v2_plan]] -- v2 scoping
- [[project_cc_sidebar_collapse]] -- sidebar collapse shipped same session
- [[feedback_wf3a_unblocked]] -- the prerequisite
- [[reference_cc_command_center]] -- CC URL + auth
