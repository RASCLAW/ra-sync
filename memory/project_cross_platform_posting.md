---
name: Cross-Platform Posting Research
description: Research findings on posting to Instagram, X, TikTok -- requirements, costs, friction
type: project
related: [project_messenger_strategy.md, project_story_rotation.md, reference_fb_stories_api.md, reference_meta_verified.md]
---

Platform expansion order (decided session 95):

1. **Facebook** -- DONE. Feed posts + stories automated.
2. **Instagram** -- Same Meta ecosystem. Needs App Review for `instagram_content_publish` (2-4 weeks). 2-step publish flow. No native scheduling.
3. **X/Twitter** -- Pay-per-use $0.01/post. Full v2 read+write. At 3 posts/day = ~P50/month.
4. **TikTok** -- Most friction. Mandatory compliance audit. Posts private until approved. Draft mode more practical initially.

**Scheduling tools:**
- Buffer free: 3 channels, 30 posts/channel/month -- good bridge
- n8n self-hosted on Oracle Cloud VM: unlimited, zero cost -- endgame
- Make.com: both free slots used, would need paid ($10.59/mo)

**Why this order:** Each step builds on existing infrastructure. FB is done. IG uses same Meta verification. X is cheap and simple. TikTok has most friction.

**How to apply:** When RA asks about expanding to other platforms, start with Instagram App Review submission.
