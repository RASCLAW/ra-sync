---
name: post-ship-scope-drift
description: "After a feature ships, pause before stacking another build session. Use the new feature in real workflow first to find what's actually missing vs what felt missing in the abstract."
metadata:
  node_type: memory
  type: feedback
  related:
    - project_schedule_tab_v1.md
    - project_schedule_tab_v2_plan.md
---

## Rule
When a build session ships (code live, validated), **stop and close out** before scoping the next build. Don't ride the build momentum into more speccing.

**Why:** Session 166 -- Schedule tab v1 went live (4 live FB posts validated, cron firing). RA immediately pivoted: "make the bot screen-aware" → "actually remove bot, add image viewer + crop" → "actually add caption chat" → "add a calendar with PH events". Each pivot was reasonable in isolation. None were tested against actual usage gaps. Result: 7-8 hours of new build scoped into `.tmp/plan_v2.md` without a single real production-flow post being made through the new v1 tool.

**How to apply:**
- After /execute completes and the closeout summary is written, default to **stop**, not "what's next?"
- If RA pivots into new scope mid-session, prompt explicitly: "Use the shipped thing for a few days first? Then we'll know what's actually missing."
- Memory rule: any new build session that follows a ship session should reference at least one observed real-use gap, not a hypothetical one.
- Exception: bug fixes / followups to the just-shipped thing are fine -- those ARE observed gaps.

## Related
- [[project_schedule_tab_v1]] -- the thing that shipped
- [[project_schedule_tab_v2_plan]] -- the thing that got scoped but not built
