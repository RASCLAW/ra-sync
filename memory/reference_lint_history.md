---
name: lint-memory-history
description: Tracks when /lint-memory was last run and results -- triggers next audit when >14 days stale
metadata: 
  node_type: memory
  type: reference
  related: 
    - project_llm_wiki_system.md
  originSessionId: 16bceab5-bd01-4dc7-a6c9-65b9c20eba8b
---

**Last run:** 2026-05-24 (session 173)

**Results:**
- 332 files audited (335 with archive)
- MEMORY.md was 343 lines (truncating); split into 3 indexes:
  - `MEMORY.md` (main, active state + references)
  - `MEMORY_BEHAVIORAL.md` (44 behavioral rules)
  - `MEMORY_CONTENT.md` (29 content generation rules)
- 2 broken index refs removed: `project_v3_best_sellers_hover.md`, `project_v3_order_redesign.md` (never existed on disk)
- 3 broken `related:` links cleaned in `feedback_css_hidden_display_override.md`, `project_v3_order_enhancements.md`, `project_v3_pdp_cart_redesign.md`
- 9 stale orphans archived: 3 Cloud Run / GitHub Pages obsolete refs, project_chatbot_live, refactor_recovery_session99, scroll_site, slate_cards, v3_order_picker, schedule_tab_v2_plan
- 4 useful orphans indexed: feedback_sequential_prompt_planning (→ CONTENT), reference_gcloud_cli, reference_token_scopes, reference_youtube_api
- Duplicate label "Chatbot Image Bank v2" resolved -- older entry relabeled "Chatbot Image Bank Schema (v1)"
- 0 contradictions found

**Previous run:** 2026-04-09 (session 95) -- 44 days gap (cadence missed)

**Next due:** ~2026-06-07 (2-week cadence)
