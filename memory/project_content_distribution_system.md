---
name: Content Distribution System (phased plumbing plan)
description: Three-phase plan to wire generated content into distribution channels via the Stage 2 manifest — story rotation, feed posting, ad insights feedback loop
type: project
related: [reference_review_dashboard.md, project_story_rotation.md, project_messenger_strategy.md, feedback_content_storage_rule.md, project_image_bank_april.md]
originSessionId: 545c83ca-7691-44bd-b4f0-4e1f98f8b71e
---
Wire the tag manifest at `contents/ready/manifest.json` into all distribution channels so generated content actually gets used.

**Why:** Diagnosed 2026-04-16 — stories recycling the same 12 images for 2+ weeks, feed posts manual, no performance feedback loop. The generation pipeline is fine; the bottleneck is distribution plumbing. RA confirmed bottlenecks (b) no routing system, (c) flying blind on performance, (d) content unused.

**How to apply:** When RA returns to content automation work, follow the phased order below. Don't skip ahead — each phase validates the manifest-driven pattern before the next channel consumes it.

## Phase 1 — Story rotation (THIS WEEK, ~20 min)

Unblocks the stories-recycling pain. Tooling already exists at `tools/facebook/story_rotation.py` (hardcoded 12 images, time-based rotation, GH Actions cron every 4 hours).

**Blockers identified:**
- `.gitignore` excludes `contents/ready/` (14 current images force-committed historically)
- Stage 1 dumps approved images FLAT in `contents/ready/`, but current rotation expects subfolders (`contents/ready/ugc/*.png`, etc.)

**Chosen approach (Option A):**
- Add exception rule in `.gitignore`: `!contents/ready/*.png` + `!contents/ready/manifest.json`
- Edit `story_rotation.py`: try to read manifest; if pool >=4 images, use it; else fall back to hardcoded list (safety net)
- Keep deterministic rotation math: `idx = (hours // 4) % len(pool)`
- At 200 tagged images × ~400KB avg = ~80MB repo growth. Git LFS later if it becomes a problem.

Not Option B (separate queue folder + sync script) — more friction, defer until bandwidth forces it.

## Phase 2 — Feed auto-poster (NEXT WEEK)

Meta Verified is approved, unblocks FB Graph API posting. Builds on Phase 1 manifest pattern.

- New tool: `tools/facebook/feed_autoposter.py`
- Reads `POST`-tagged images from manifest
- Posts 1/day at fixed time (suggest noon MNL — matches audience pattern from boosted post data)
- Skips if queue empty (graceful)
- Manual first week to validate, then cron via GH Actions

## Phase 3 — Meta insights feedback loop (AFTER PRODUCTION DATA WEEK)

Closes the "flying blind" gap. Existing `tools/meta_ads/` already pulls insights.

- Weekly cron reads Meta ad insights (CTR, comment rate, reach)
- Winners (top quartile) → auto-add `AD` tag in manifest
- Losers (bottom quartile of tested) → auto-add `ARCHIVE` tag (removed from active pools)
- Dashboard view: tag distribution + performance per tag

## Command Center tie-in

The Stage 1 + Stage 2 dashboards are accidentally the Command Center prototype from the backlog. Once Phase 3's analytics view exists, unify into `/dashboard` with shared nav: [Review] [Tag] [Dashboard]. That becomes the RAS Creative portfolio piece — AI content pipeline + human-in-the-loop QA + data-driven distribution.

## Status (as of 2026-04-16)

- Stage 1 + Stage 2 live on review.duberymnl.com + tag.duberymnl.com
- 239-image backlog being reviewed (RA async during lunch + after work)
- Phase 1 plumbing not yet implemented — waiting on RA to finish review so the STORY-tag pool is meaningful
