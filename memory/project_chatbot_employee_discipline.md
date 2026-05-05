---
name: Chatbot Employee Discipline Upgrade
description: Session 127 added 7 guardrails so the bot behaves like a disciplined customer service employee -- knows when to stop pitching and hand off. TURN_CAP=10, policy one-shot, complaint + pushback detectors.
type: project
related:
  - project_chatbot_behavior_aligned.md
  - project_chatbot_recovery_complete.md
  - feedback_metro_manila_only.md
  - feedback_handoff_continuation.md
  - reference_chatbot_admin_endpoints.md
originSessionId: f2b83343-be79-40bb-b8a6-851c6856aee3
---
**Shipped 2026-04-16 (session 127).** Driven by the Alkabir incident — a Tawi-Tawi organic follower who was a provincial buyer the bot spiraled on: 27 messages, bot repeated the "provincial prepayment required" policy 9 times, claimed to send a QR image 5+ times without actually attaching it, never detected "2beses na ako naloko" as a trust objection, never handed off. Customer eventually said "try ko na lang sa tiktok" and walked.

**Design philosophy shift:** Bot is a disciplined employee, not a sales-obsessed retry loop. It doesn't have to close every deal — only the easy/urgent ones. Anything else hands off early. Policies are stated ONCE, not re-litigated.

**7 guardrails now stacked (in order of precedence in `process_message`):**

1. **Human takeover** — RA types in Page Inbox → Meta echoes with `app_id != META_APP_ID` → bot flags handoff, goes silent. Released via `POST /release/<sender_id>`.
2. **Complaint detector** (pre-Gemini) — ~30 PH trust/scam/deflection phrases ("naloko", "2beses", "ayaw ko na", "try ko na lang sa tiktok/shopee/lazada"). Short-circuits Gemini, bridge line + handoff + TG ping.
3. **Policy pushback** (pre-Gemini) — if a delivered policy is pushed back on, short-circuit. Two policies tracked: `prepay_provincial` (COD pushback keywords) and `no_discount` (DUBERY50/tawad keywords).
4. **Phantom QR injector** (post-Gemini) — regex catches "here's our QR"/"QR code"/etc. in reply text; if `support-instapay-qr` isn't in `image_keys`, inject it so image actually sends.
5. **Turn cap** (post-Gemini) — `TURN_CAP=10` assistant replies without `order_complete=true` → override reply + handoff. Env-configurable via `CHATBOT_TURN_CAP`.
6. **Loop guard** (post-Gemini) — 3 consecutive replies with same "theme signature" (prepay+cod_metro+ask_model etc.) → override + handoff. Runs only when turn cap hasn't fired.
7. **first_name persistence** — when Gemini extracts a name, first word stamped on `conv.metadata.first_name` for future replies.

**Key decision: TURN_CAP=10, not 6.** 6 was too tight — simple buyer closes in 5 turns, browsing buyer 7-8, chatty buyer 10+. Cap is a last-line backstop (other detectors catch specific failures earlier), should be lenient. Misfired handoff on an in-progress sale is worse than a missed handoff (recoverable).

**New handoff reasons:** `human_takeover`, `complaint_detected`, `policy_loop`, `turn_cap`, `bot_in_loop`.

**Also shipped same session:**
- Ad referral capture (Phase 1) — webhook stores `source_ad_id/source_ref/source_type` on conversation metadata, logs to `.tmp/referral_log.jsonl`. Requires ref tagging on ads in Ads Manager to populate (not done yet).
- `/flag/<sender_id>` and `/release/<sender_id>` admin endpoints.
- Alkabir flagged manually — bot silent on him, RA taking over.
- **Echo logging**: RA's manual replies from Page Inbox are now captured to `conversation_store` AND CRM as `role=assistant, intent=manual`. Closes the invisibility gap on manually-closed sales. Stats: `stats.manual_replies_logged`.
- **24h time-decay handoff release**: on next customer message, stale handoff flags auto-clear after `HANDOFF_DECAY_HOURS` (default 24). Also resets reply-signature FIFO. Stats: `stats.handoff_auto_released`.
- **18h proactive nurture scanner**: daemon thread, wakes every `CHATBOT_NURTURE_SCAN_SECONDS` (default 1800), fires ONE follow-up per customer when 18-23h silent AND showed `inquiry`/`order` interest AND not handed-off/sold/nurtured. Inside Meta 24h window with 1h safety buffer. 3 rotating templates with `{name}` substitution. Stats: `stats.nurtures_sent`, `stats.nurture_failed`.
- **Rename**: `cloud-run/` → `chatbot/` (via `git mv`). Task Scheduler re-registered, 8 path refs updated, log renamed to `.tmp/chatbot-server.log`. Some memory files still reference `cloud-run/` — catch on next `/lint-memory` sweep.
- **Portfolio-worthy README**: [chatbot/README.md](chatbot/README.md) -- 14 sections, architecture diagram, guardrail table, env vars, admin endpoints, known gaps, roadmap.
- **`/mark-sale/<sender_id>` endpoint** -- manual-close capture: accepts JSON/form/query, validates items+total, creates CRM order row via `create_order`, stamps `order_recorded=True` + `last_order_id/total/at` on conv metadata, flags handoff, appends a `[manual sale marked -- ...]` transcript line. Double-sale guard via 409 unless `force=true`. Optional `upsert_lead` when name/phone/address/landmarks provided. Stats: `manual_sales_marked`.
- **`/conversations` v2 admin UI** -- rich badges (handoff+reason, order+id+total, policy chips, source ref/ad_id, nurture, last 3 intents), 11-counter stat bar, per-row RELEASE/FLAG/MARK-SALE buttons wired via AJAX fetch, inline sale form (items + total + payment + note), toast notifications, first_name display.
- **Ad-aware openers Phase 2** -- `chatbot/ad_registry.json` with 15 entries (9 per-variant + 3 per-series + 3 generic), `conversation_engine.get_ad_context()` lookup with ref-first ad_id-fallback, `generate_reply(..., ad_context=...)` kwarg injects `AD_CONTEXT:` + `AD_PRODUCT_FOCUS:` into Gemini's system-prompt on FIRST CONTACT only (turn 2+ skips hint). Logs `ad_context_applied` event. Unknown refs safely fall back to generic SALES TEMPLATE.
- **System prompt softening (disciplined-employee voice)** -- new REPLY CLOSES section forbids reflexive "which model?" close + "policy + promo + question" stacking. Default neutral closes ("Just let me know po when ready"), probe only on undecided-new OR mid-order-collection. PROMO UPSELL now "ONCE per conversation" rule. "ok/sige/noted" rule softened (no "Anything else po?" reflex). 2 new JSON examples demonstrate neutral closes. Live Gemini tests validated 3 scenarios (provincial policy, decline, sizing question).
- **`chatbot/README.md` caught up (savepoint 3, 02:00 UTC+8 2026-04-17)** -- now documents all of tonight's shipments: Phase 2 ad-aware + `/mark-sale` + `/conversations` v2 + manual-close CRM capture + ad_registry.json in project structure + all new env vars + updated Known Limitations + refreshed Roadmap (multi-tenancy, `/reload-registry`, insistence detection, Phase 3 A/B, client-pitch package). State is portfolio-shippable as-is.

**How to apply:**
- When RA describes a chatbot misbehavior pattern, think *which guardrail should catch it* before adding another detector. 7 is already a lot.
- Policies are first-class citizens now. New non-negotiable rules should get a `POLICY_DEFINITIONS` entry in `security.py` with delivery markers + pushback keywords.
- `TURN_CAP` tune-down needs justification — loose > strict by default.
- Alkabir convo is the portfolio example: *"The hard part wasn't making the bot talk. It was teaching it when to stop."*

**Remaining (next session candidates):**
- `/conversations` admin view: surface `policies_delivered`, `source_ad_id`, and a "Release" button.
- System prompt pass for "employee mindset" — soften the reflexive "which model/color?" close at end of every reply.
- Insistence detection — same customer objection topic 2x → handoff (more general than policy pushback).
- Ad registry mapping `ad_id` → tailored opener (Phase 2 of ad-aware).
