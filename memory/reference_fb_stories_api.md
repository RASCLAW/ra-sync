---
name: Facebook Stories API
description: How to post stories to FB Page via Graph API -- endpoints, specs, limitations
type: reference
related: [project_cross_platform_posting.md, project_story_rotation.md]
---

## Endpoints

**Photo story (2-step):**
1. `POST /{page-id}/photos?published=false` with file upload -> returns photo_id
2. `POST /{page-id}/photo_stories?photo_id={id}` -> publishes

**Video story (3-step):** start upload -> upload file -> finish

## Key Details

- **Permissions needed:** `pages_manage_posts`, `pages_read_engagement`, `pages_show_list` (DuberyMNL has all)
- **No scheduling via API** -- publishes immediately. Use external cron.
- **No text/sticker overlays** -- all text must be baked into image before upload
- **No media reuse** -- same photo_id can't be used for both feed post and story. Upload the file twice.
- **Image specs:** 1080x1920 (9:16), max 10MB. JPEG/PNG/BMP/GIF/TIFF.
- **Video specs:** MP4, 9:16, 1080x1920, 3-90 seconds, H.264/H.265
- **Stories expire after 24 hours**
- **Shares the 25 posts/day limit** across all post types (feed + stories)
- **API version:** v25.0 (latest as of Apr 2026)
