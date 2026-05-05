---
name: Story Rotation System
description: Automated Facebook story posting via GitHub Actions -- 12 images, every 4 hours, time-based rotation
type: project
related: [project_cross_platform_posting.md, reference_fb_stories_api.md, feedback_story_pool_stale_refs.md]
originSessionId: 403031e3-ac94-4d66-aeae-dc6953510459
---
DuberyMNL has automated story posting via GitHub Actions.

- **Workflow:** `.github/workflows/story-rotation.yml`
- **Script:** `tools/facebook/story_rotation.py`
- **Schedule:** Every 4 hours (cron `0 */4 * * *`) = 8AM/12PM/4PM/8PM/12AM/4AM PHT
- **Images:** 12 images hardcoded in script (5 model shots, 5 UGC, 2 brand bold)
- **Rotation:** Time-based index `(hours_since_epoch / 4) % 12` -- stateless, no file needed
- **Full cycle:** 48 hours, then repeats
- **GitHub Secrets:** META_PAGE_ACCESS_TOKEN, META_PAGE_ID set on RASCLAW/DuberyMNL
- **API:** Facebook Graph API v25.0, 2-step photo story flow (upload unpublished -> publish as story)

**Why:** Stories get prime feed placement, 24hr auto-expiry means fresh content constantly. GitHub Actions runs even when laptop is off.

**How to apply:** To add/change images, update the STORY_IMAGES list in story_rotation.py, commit, push. To change frequency, edit the cron schedule in the workflow file.
