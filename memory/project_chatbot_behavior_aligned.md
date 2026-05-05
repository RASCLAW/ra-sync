---
name: Chatbot Behavior Aligned
description: Session 125 aligned all critical chatbot behavior dimensions -- first-contact, handoff, formatting, persistence, image strategy, auto-DM
type: project
related:
  - project_chatbot_employee_discipline.md
  - project_chatbot_recovery_complete.md
  - feedback_handoff_continuation.md
  - feedback_worker_ping_rule.md
  - feedback_metro_manila_only.md
  - reference_cloudflare_worker_fallback.md
  - reference_chatbot_live_path.md
  - reference_chatbot_admin_endpoints.md
originSessionId: e90a13f6-dc4b-4c54-a15a-1f2aab344611
---
**Status:** All critical chatbot behavior dimensions aligned as of session 125 (2026-04-16).

**Covered:**
- **First-contact:** SALES TEMPLATE fires on pricing/greeting triggers. Album URL included. Preserves image-aware path (no template on screenshots/product asks).
- **Layer coordination:** Worker=fallback FAQ (5 intents), Flask=primary Gemini. Transparent forward when origin up.
- **Handoff:** Dedup initial ping, bot keeps replying, 🔥 urgent follow-up detection.
- **Formatting:** Multi-point replies broken into blocks (MULTI-POINT REPLIES section in SYSTEM_PROMPT).
- **Persistence:** conversation_store persists to disk, survives restarts. Returning customers keep full history.
- **Image strategy:** Hero/packaging shots (11/11), lifestyle (6/11). Model shots temporarily removed (RA providing new). 2-image combo planned (product-only + packaging) pending CDN upload.
- **Comment auto-DM:** Nurture message + 10 keywords + album link via Meta Business Suite. Funnel: comment → DM → Gemini handles.
- **Speed/boundaries/error:** Implicitly covered by existing SYSTEM_PROMPT rules.

**Remaining (non-urgent):**
- Ad-aware chatbot Phase 2 (tailored openers per ad) — Phase 1 capture shipped session 127, waiting on ref tagging in Ads Manager
- 2-image combo needs product-only kraft shots uploaded to CDN
- New model shots from RA
- Auto-responder code rebuild (comment_responder.py) — future session

**Session 127 (2026-04-16) extension:** Employee-discipline upgrade added 7 stacked guardrails. Turn cap, policy one-shot, complaint detector, loop guard, phantom QR injector, human takeover, first_name persist. See `project_chatbot_employee_discipline.md`.

**How to apply:** Before adding new chatbot behavior, check if it fits within the aligned framework above. The SYSTEM_PROMPT in conversation_engine.py is the source of truth for behavior rules.
