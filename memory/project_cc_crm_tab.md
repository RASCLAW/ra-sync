---
name: project-cc-crm-tab
description: Command Center CRM tab -- Sheets data + Meta page analytics, routes confirmed live
type: project
related: [reference_cc_command_center.md, feedback_meta_requests_comma_encoding.md, project_meta_catalog.md]
---

CRM tab shipped in Session 156 (2026-05-15). All 4 routes live and confirmed returning data.

**Routes added to `command-center/app.py`:**
- `GET /api/crm/summary` -- aggregate stats: total_leads, status_counts (Hot/Warm/Cold/Converted), total_orders, orders_today, total_revenue
- `GET /api/crm/leads` -- all leads newest-first (cap 100), keyed by LEAD_COLUMNS from crm_sync.py
- `GET /api/crm/orders` -- all orders newest-first (cap 100)
- `GET /api/analytics/page` -- two-call Meta approach: `/me?fields=fan_count,...` (always live) + `/insights` for time-series (period=week; only 3 metrics work: page_impressions_unique, page_post_engagements, page_views_total)

**Current data (2026-05-15):**
- 33 leads: 19 Warm, 13 Cold, 1 Converted, 0 Hot
- 1 order: ORD-20260419-130654, ₱1,198, Pending (Kingpin Dela Cruz)
- 2,098 followers, 23 talking about

**Frontend files:**
- `command-center/templates/tabs/crm.html` -- tiles + analytics card + leads/orders tables
- `command-center/static/js/crm.js` -- loads on tab:activated, refresh button
- `command-center/static/css/main.css` -- .crm-* and .badge-* classes appended (v=crm1)

**Auth pattern:** reuses `chatbot.crm_sync._get_service()` -- requires `token.json` locally (re-authed 2026-05-15).

**Why:** Step 3 of handoff: wire CC with live CRM data. Steps 1 (FB analytics) and 3 (website visits) also addressed -- analytics partially wired, website visits pending.
