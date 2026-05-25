---
name: cc-marketing-tab-v2
description: 2026-05-25 session 174 add — CC Marketing tab rewritten from staging-only UI to analytics dashboard; 6 sections (snapshot, adsets, ads leaderboard, pixel, trend, page+attention), 2 new pullers (live_meta + pixel_stats), single /api/marketing/summary endpoint, manual refresh button, no mutations
metadata:
  type: project
related:
  - project_cc_dashboard_overhaul_2026_05_25
  - feedback_meta_pixel_stats_shape
  - feedback_thumb_url_for_grids
  - reference_meta_use_cases_picker
  - project_messenger_strategy
---

Built atop the dashboard overhaul block of session 174. Marketing tab previously was a 3-column creative-staging UI; rewritten as analytics-first with the staging UI collapsed into a `<details>` section at the bottom.

**Why this shape:** RA's handoff said "show facebook ads analytics, and pixel data. should show the exact ads being used, the budget, etc. so that i can learn faster what's happening." Daily-use surface is analytics; staging happens weekly at most. One screen, analytics on top, staging collapsed → right weight without losing the workflow.

## 6 analytics sections

1. **Account snapshot strip** — 6 tiles over rolling 7d (Spend / Impressions / Link clicks / LPV / Messages / Pixel Purchases) with derived sub-stats (CPM, CPC, Cost/LPV)
2. **Adsets running** — table joining insights performance with live-meta budgets/statuses (daily budget + status pill + 7d spend + CTR + Cost/LPV + LPV + msgs)
3. **Live ads leaderboard** — sortable table with Meta creative thumbnails (`creative.thumbnail_url`). Default sort: Cost/LPV ascending (cheapest first). Color cues: CTR green if ≥1.25× batch avg, red if ≤0.6×, warn if <0.85×; Cost/LPV green if ≤0.75× batch avg, red if ≥1.4×, warn if ≥1.15×.
4. **Pixel events** — PageView / ViewContent / AddToCart / Purchase counts + funnel bars (pct of PageView). Gap callout shows when sheet_orders > pixel_purchases (hidden when Pixel sees more or equal).
5. **Daily trend** — pure SVG line chart (no Chart.js dep). Spend (solid orange) + LPV (solid green) + CTR (dashed gold) over 14 days. Recent 3-day vs first 3-day spend trend annotation.
6. **Page + Needs attention split** — FB Page metrics on left (fans, talking-about, 28d page impressions + engagements + views from existing `/api/analytics/page`). Derived attention rows on right: pause candidate if any active ad has CTR<1% AND spend>₱20; top spender = best Cost/LPV among ads with ≥3 LPV; watching = below-avg CTR with ≥3 LPV; pixel-vs-sheet gap row if `unattributed > 0`.

## New tools (`tools/meta_ads/`)

- **`pull_live_meta.py`** — Meta Marketing API. Fetches `/{campaign}/adsets?fields=...daily_budget,status,effective_status...` and `/{campaign}/ads?fields=...creative{id,thumbnail_url,image_url}`. Budgets returned in centavos by Meta; divides by 100 → human-readable pesos in JSON. Writes `.tmp/marketing_live_meta.json`.
- **`pull_pixel_stats.py`** — Meta Pixel `/stats` endpoint. Site-wide funnel events (not just ad-attributed). Writes `.tmp/pixel_stats.json`. Same `META_ADS_ACCESS_TOKEN` scope as `pull_insights.py` — no extra permission needed. Pixel ID env var `META_PIXEL_ID` (defaults to `1513349880261420`).
- **Modified `pull_insights.py`** — added `--output` flag so the 14-day daily breakdown writes to `.tmp/ad_insights_daily.json` without clobbering the 7-day summary at `.tmp/ad_insights.json`.

## Endpoints (`command-center/app.py`)

- **`GET /api/marketing/summary`** — single consolidated read; reads all 4 cache files, joins insights-adsets ↔ live-meta-adsets by `adset_id` and insights-ads ↔ live-meta-ads by `ad_id`. Derives needs-attention items in Python. No Meta API calls from this Flask process. Returns ~30KB JSON with `cache.{file}_mtime` for each source so UI can display freshness.
- **`POST /api/marketing/refresh`** — subprocess chain (sequential, ~10-15s):
  1. `pull_insights.py --quiet --days 7` (7d summary)
  2. `pull_insights.py --quiet --days 14 --daily --output .tmp/ad_insights_daily.json` (14d daily)
  3. `pull_live_meta.py --quiet`
  4. `pull_pixel_stats.py --quiet --days 7`

  Returns `{ok, steps: {step_name: {ok, exit, stderr}}}` so UI can surface partial failures.

## Frontend

- **`command-center/templates/tabs/marketing.html`** — full rewrite. Staging UI preserved in `<details id="mkt-staging-details">` at the bottom, lazily initialized on first `toggle` event so its presets+content loads only when opened.
- **`command-center/static/js/marketing.js`** — analytics fetch+render + sortable ads table + SVG trend chart + gap callout. Staging logic kept (rename of state vars from local `state` to `S` to avoid collision with analytics state `A`). Sort default: `cost_per_lpv` ascending for ads table.
- **`command-center/static/css/main.css`** — `.mkt-*` analytics styles + responsive breakpoints. CSS bumped `?v=mkt4` in shell.html; marketing.js also `?v=mkt4`.

## Safety rule wired

- Refresh button is read-only (no mutations).
- No ACTIVE/PAUSED toggle endpoints exist in this batch.
- Existing PAUSED-only Stage flow preserved untouched in the collapse.
- Mock-first workflow followed: built `.tmp/marketing-mock.html` in real CC light theme + realistic numbers from cached insights, RA approved, then wired.

## Live data validated at ship (2026-05-25 ~05:20 PHT, 7d window)

- ₱882 spend, 33,032 impressions, 506 clicks (CPC ₱1.74), 251 LPV (Cost/LPV ₱3.51), 4 messages, 1 omni_purchase
- 2 active adsets, ₱70/day budget each (Bespoke UGC May2026 + Brand Graphics May2026)
- 24 active+spending ads (25 insights, 37 total in account; non-spenders filtered)
- All ads rendered creative thumbnails from Meta CDN
- Pixel (site-wide 7d): PageView 1309, ViewContent 137 (10.47%), AddToCart 9 (0.69%), Purchase 4 (0.31%)
- Sheet orders 7d: 3 (excl. CANCELED) — Pixel sees more than Sheet, so gap callout hidden
- Top spender by Cost/LPV: bespoke-outback-red-graphic-a at ₱2.01/LPV (₱12 spent, 6 LPV)
- No pause candidate currently meets threshold (no active ad with CTR<1% AND spend>₱20)

## Follow-ups (not blocking)

- Ads table shows all rows (24 currently); no truncation yet. Easy `slice(0, 10)` + "show all" toggle when noisy.
- Refresh is 4 sequential subprocesses ~10-15s; could parallelize with `concurrent.futures` but per-step errors are clearer sequential. Defer until it actually hurts.
- Gap callout currently hidden because Pixel sees 4 vs Sheet's 7d 3 — might be Pixel double-firing on the v3 site or test orders; revisit once the 2026-05-25 `utm_content={{ad.id}}` attribution wiring sees real ad-driven orders.
- Per-ad creative previews are 44px thumbs in a table cell — lightbox-on-click would be a nice add when reviewing creative variants.
- No ad freshness/fatigue calc yet (declining CTR over time). Possible Phase 3 add using the 14-day daily breakdown joined with per-ad data.

## See also

- `feedback_meta_pixel_stats_shape.md` — the nested-bins gotcha that cost one debug round
- `project_cc_dashboard_overhaul_2026_05_25.md` — sibling work in the same session
- `feedback_thumb_url_for_grids.md` — perf rule applied to the creative thumb cells
- `reference_meta_use_cases_picker.md` — same Meta token that powers this work
