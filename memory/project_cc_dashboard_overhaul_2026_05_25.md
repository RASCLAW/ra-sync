---
name: cc-dashboard-overhaul-2026-05-25
description: Big CC dashboard upgrade session -- CRM tab production-ready (5 tiles + click-detail + Page Analytics enabled), Schedule tab gained queue-card detail modal + edit endpoint + FB-style preview + column collapse + zoom slider + unified favorites, AI Suggest agent rewritten as thinking skill with chat history persistence, Worker first-touch gate shipped
metadata:
  type: project
related:
  - project_orders_consolidation_2026_05_24
  - project_ai_suggest_skill_rewrite
  - reference_meta_use_cases_picker
  - feedback_thumb_url_for_grids
  - feedback_takeover_via_dashboard
  - reference_schedule_chat_vs_agent_chat
  - project_image_bank_overhaul
  - project_cc_marketing_tab_v2
---

Session 174 (2026-05-25) -- big leveling-up day for the CC dashboard. Started from a complaint about Worker fallback noise + ended with a production-grade CRM tab, editable Schedule queue, and a smarter AI Suggest agent. Highlights below; the actual code is the source of truth.

## What shipped

**Cloudflare Worker -- first-touch gate.** Deployed `dubery-chatbot-fallback` v `625f9589`. When the laptop is asleep, the worker only auto-replies to senders it hasn't seen in 24h. Active conversations get silence (RA handles them), while fresh first-touches still get an FAQ template. `order_intent` bypasses the gate and pings TG. Seen ledger stamped on every inbound via Workers KV. See [chatbot/cloudflare-worker/worker.js](../../../projects/DuberyMNL/chatbot/cloudflare-worker/worker.js).

**CRM tab -- 5 tiles, click-detail, Page Analytics live.**
- Reads moved to v3 Orders sheet ([[orders-consolidation-2026-05-24]]). 5 orders / P4,339 was undercount (CC was reading stale CRM > Orders); now shows live state.
- 5 tiles: Total Leads / Total Orders / Total Revenue (excl. cancelled) / Orders (24h, rolling) / Units Sold (30d, summed from Qty col excl. cancelled). Each has a tooltip explaining source + rule.
- Click any row in Leads or Orders tables -> modal with full detail (name, phone, address, items, source ad_id, notes, status, etc).
- Page Analytics tiles now populated (Reach 283K / Engagements 8.7K / Page Views 3.9K over 28d) -- token had to be re-issued with `read_insights` scope, see [[reference_meta_use_cases_picker]].
- Refresh button passes `?fresh=1` to bypass the 30s cache so it actually refetches.
- Bearer-token Sheets reader (urllib3, not googleapiclient) + 30s TTL cache: cold load ~2.7s, warm load ~50ms (50x faster). Dodges the httplib2 SSL flakes from RA's home network (see [[reference_googleapi_httplib2_fallback]] for the pattern).

**Schedule tab -- queue card detail modal + edit upcoming posts.**
- Click any Upcoming/Posted/Failed card -> FB-styled detail modal (capped 560px feed-width, real FB grid logic for 1/2/3/4 photos + "+N" overlay, collage layouts preserved).
- For Upcoming cards: Edit button -> caption becomes textarea + scheduled time becomes datetime-local input -> Save commits via new `POST /api/schedule/edit` endpoint. Validates future-PHT + status=APPROVED before patching `queue.json`.
- Cancel post + Close buttons in modal footer.
- Each column (Upcoming/Posted/Failed) collapses to 4 newest items by default; "Show N more" button reveals the rest.
- Image grids in modal use `/api/thumb?w=480` (~15KB each) instead of full PNGs (~1.5MB) -- detail modal open dropped from ~6MB to ~60KB. See [[feedback_thumb_url_for_grids]].

**Schedule picker (image bank modal) -- runs/ folder + cleaner thumbs + zoom + resize.**
- `/api/schedule/image-bank` now scans `contents/runs/{timestamp}_bespoke/` too. Same gap as the Image Bank tab had (59 bespoke pipeline outputs were invisible). Memory [[reference_cc_bespoke_pipeline]] documents the folder structure.
- Filename label + "DRAFT" pill hidden on thumbnails for visual breathing room (CSS `display:none`).
- Zoom slider in toolbar (80-280px, persisted to localStorage key `sched-bank-zoom`). Same pattern added to Image Bank tab (100-320px, key `ib-zoom`).
- Picker modal default width 1400px, resizable via `resize: both` CSS handle (drag bottom-right corner).
- Schedule preview modal + Image Bank lightbox both widened to 1300px max.

**Favorites unified.** Image Bank tab and Schedule picker now share the server-side store at `contents/ready/favorites.json` via `/api/schedule/favorites`. Image Bank used to use localStorage `ib-favorites`; migration runs on first load, then clears the legacy key. Heart on one surface shows on both.

**AI Suggest agent -- rewritten as a thinking skill.** System prompt in `_build_sched_chat_system_prompt()` (command-center/app.py). Key shifts:
- 4-step framework: READ THE IMAGE -> MATCH REGISTER -> 5-8 OPTIONS (labels emerge from image, not fixed menu) -> PICK + INVITE NEXT MOVE.
- Captures brand voice evolution explicitly: "same affordable 499, but imagery has leveled up from local DTC snapshots toward premium editorial -- voice should follow the image without losing accessibility."
- `duberymnl.com` CTA cadence baked in: ~1 of every 3-4 captions includes the URL, weighted to product-forward angles.
- Iteration rule: when RA pushes a direction in followup, deepen the thread (don't reset to generic menu).
- See [[project_ai_suggest_skill_rewrite]] for the full design intent.

**Chat history persistence.** Every AI Suggest brainstorm saves to `.tmp/sched_chat_history/<sid>.json` after each turn. Survives CC restart (Claude resume_id preserved). New `GET /api/schedule/chat/sessions` endpoint lists past brainstorms with previews. RA's audit trail for "how we came up with the final caption".

**Emoji picker in Schedule chat composer.** Curated 5 groups, 40 emojis -- shades / outdoor / action / shop / reactions. Click inserts at cursor position in textarea.

**Sheet writes.** Jeffrey Arragona row -> col K = `DELIVERED`. Apollo Planas row -> col K = `CANCELED`. Both confirmed via RA.

## Decisions made

- **Option C consolidation finalized.** Orders write -> v3 Orders sheet exclusively. CRM > Orders tab stays as legacy archive (1 row from Apr 19, untouched).
- **Browser bookmarklet killed.** RA prefers `/conversations` dashboard for manual takeover. See [[feedback_takeover_via_dashboard]].
- **Token strategy.** Use Cases panel is the new home for App permissions; `read_insights` is NOT under "pages_" prefix; new Page token is permanent (`expires_at: 0`). [[reference_meta_use_cases_picker]] for the dance.
- **Revenue excludes cancelled** but Total Orders counts all statuses. Subtitle on revenue tile says "excl. cancelled" for clarity.
- **Orders (24h) over Orders Today.** Rolling 24h window so late-night orders don't roll off at midnight.
- **Column collapse default 4.** Posted column was sprawling (9 items). 4 keeps the page tight; Show More reveals.

## What's NOT done

- Cloudflared NOT installed as Windows service. Currently launched via `Start-Process -WindowStyle Hidden` (survives terminal closures, but dies on reboot until manual relaunch). Backlog item.
- `message_echoes` Meta subscription -- echo handler exists in chatbot code ([chatbot/messenger_webhook.py:1151](../../../projects/DuberyMNL/chatbot/messenger_webhook.py#L1151)) but the App webhook subscription doesn't include the field. RA can curl-enable it via the Page subscribed_apps endpoint when ready; until then manual takeover stays a `/conversations` button click.
- Meta Pixel NOT installed on duberymnl.com / v3 site (separate workstream).
- All session changes still local; not yet committed.

## Restarts done this session

CC restarted ~7 times across the session for Python code changes. Each restart caused ~3-5 sec downtime; no gen subprocess was killed (verified by process listing before each kill). Worker restarted once for the first-touch gate deploy.

## File touch summary (changes still local)

- `chatbot/cloudflare-worker/worker.js` -- first-touch gate
- `chatbot/crm_sync.py` -- orders write to v3 schema, items parser, `_get_creds` helper
- `chatbot/messenger_webhook.py` -- MARK SALE form gained Name/Phone/Address; route forwards them
- `command-center/app.py` -- bearer-token Sheets reader + TTL cache; v3 reads for `/api/crm/*`; revenue excl. cancelled; rolling 24h orders; units_sold_30d; `/api/schedule/edit`; `/api/schedule/chat/sessions`; chat history persistence; AI Suggest prompt rewrite; runs/ scan in both image-bank endpoints
- `command-center/templates/tabs/crm.html` -- 5 tiles with tooltips; modal container
- `command-center/templates/tabs/schedule.html` -- emoji picker; zoom slider; queue detail modal container
- `command-center/templates/tabs/image_bank.html` -- zoom slider
- `command-center/static/js/crm.js` -- v3 schema mapping; click-row detail modal; tile data wiring
- `command-center/static/js/schedule.js` -- queue card click -> detail modal; edit flow; column collapse; zoom slider
- `command-center/static/js/schedule_chat.js` -- emoji picker logic
- `command-center/static/js/image_bank.js` -- server-side favorites; thumb URL grid; zoom slider; localStorage migration
- `command-center/static/css/main.css` -- modal styles; FB card width cap; tile-grid auto-fit; resize handle; zoom slider styles
- `.env` -- META_PAGE_ACCESS_TOKEN swapped (backup at `.env.bak-20260524-012912`)
- `decisions/log.md` -- orders consolidation decision logged
