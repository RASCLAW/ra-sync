---
name: wf3a-feed-publish-unblocked
description: "2026-05-20 session 163 -- CLAUDE.md \"WF3a blocked on Meta verification\" note is stale. PAGE_ACCESS_TOKEN has pages_manage_posts scope; feed publishing works via Graph API."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - project_feed_scheduler.md
    - project_first_closed_order.md
  originSessionId: eca7da81-9a6e-484a-960a-59e08728160d
---

## Rule
The note "WF3a: Auto-post to Facebook (blocked on Meta verification)" in `c:/Users/RAS/projects/DuberyMNL/CLAUDE.md` is stale. **Feed publishing works.** The current `META_PAGE_ACCESS_TOKEN` already has `pages_manage_posts` scope.

**Why:** Tested 2026-05-20 via a one-shot `.tmp/probe_fb_post.py` that POSTed to `graph.facebook.com/v21.0/{page_id}/photos` with `published=true` and a brand callout image. HTTP 200, post landed live on the Page (post_id `111349974035733_1423573109796641`). RA confirmed visually and left the post up.

**How to apply:**
- Stop treating WF3a as a blocker for any feed-post automation work.
- Update `CLAUDE.md` pipeline data-flow section to remove the "blocked on Meta verification" qualifier on WF3a (deferred to next session).
- `tools/facebook/schedule_post.py` (which has been sitting unused) is already wired for this -- it was likely never tested because of the stale note.
- This also means future RAS Creative client chatbots can include feed-post automation in their pitch, not just messaging.

## What's still actually blocked
Nothing related to feed posting. Story posting was always working (`story_rotation.py` runs on cron). The only Meta-side capability the Page hasn't validated is anything requiring app review beyond `pages_manage_posts` -- e.g., Instagram Graph API posting, ads automation needing `ads_management`, etc.

## Related
- [[project_feed_scheduler]] -- the use case that exposed the stale note
- [[project_first_closed_order]] -- earlier hybrid architecture decision context
