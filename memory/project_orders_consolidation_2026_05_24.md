---
name: orders-consolidation-2026-05-24
description: Single source of truth for DuberyMNL sales -- DuberyMNL Orders sheet receives both v3 PDP form orders and chatbot /mark-sale closes; CRM > Orders tab deprecated
metadata:
  type: project
related:
  - reference_dubery_orders_sheet.md
  - reference_dubery_crm_sheet.md
  - project_chatbot_live.md
  - project_cc_crm_tab.md
---

**Decision date:** 2026-05-24. Triggered by Jeffrey Arragona delivery confirmation + RA asking "where are we recording our sales history?" The audit found 5 real sales (P4,339) in the v3 Orders sheet vs 1 stale Pending row in CRM > Orders tab, and CC CRM tab was reading the wrong sheet so it undercounted as "1 order".

**Why:** Two sales-tracking sheets meant no single screen showed all revenue. Every real sale to date came through the v3 PDP form -> Apps Script webhook -> "DuberyMNL Orders" sheet. Chatbot /mark-sale was plumbed but barely used (0 adopted closes), so consolidation cost = ~zero existing data to migrate.

**How to apply:**

1. **All future closed sales land in the same sheet** — sheet id `1vS-yuFWovqHYWrFte4QXJLtH3Q2-BRDi6i9P4-vXbkA`. Schema (cols A-J): Timestamp | Name | Phone | Address | Items | Qty | Delivery Fee | Total Amount | Notes | Ad ID. Cols K/L stay manual per [[reference_dubery_orders_sheet]].

2. **Ad ID column is the origin tag** — `order_form` = v3 PDP webform; `chatbot_mark_sale` = manual close via /conversations dashboard. Future origin tags (e.g. Shopee, TikTok Shop) get their own Ad ID labels.

3. **Items format is enforced** — "{Series} – {Color}" with EN-DASH (U+2013), one per line within the cell. `chatbot/crm_sync.py::_parse_items()` handles the conversion from the dashboard's free-form "Bandits Green x1, Outback Red x1" input.

4. **Customer fields auto-filled** — name/phone/address come from the dashboard form when provided. If blank, `_load_lead_contact()` reads CRM > Leads tab as fallback. Practical implication: RA should paste name/phone/address from Page Inbox profile into the MARK SALE form because chatbot rarely extracts them well (Jeffrey's Leads row showed all 3 blank).

5. **Payment method lives in Notes** — v3 schema has no Payment column. `create_order` always appends "Payment: COD" (or whatever) to the Notes cell. CC reader (`/api/crm/orders`) parses it back out for display.

6. **CRM > Orders tab is dead** — `1wVn9...cewA` Orders tab no longer written or read. Apr 19 ORD-20260419-130654 row (Rasta R+B, P1,198, Pending) left in place per RA = test/dead lead. CRM sheet's Leads / Conversations / Lead Score Log tabs stay active for chatbot operations.

**Files changed in this consolidation:**

- [chatbot/crm_sync.py](../../../projects/DuberyMNL/chatbot/crm_sync.py) — added `ORDERS_SHEET_ID`, `_parse_items()`, `_load_lead_contact()`, `_v3_timestamp()`; rewrote `create_order()` for v3 schema; kept old `SHEET_ID` for leads/conversations
- [chatbot/messenger_webhook.py](../../../projects/DuberyMNL/chatbot/messenger_webhook.py) — MARK SALE form gained Name/Phone/Address inputs; `doMarkSale` JS sends them; `/mark-sale` route passes them to `create_order`
- [command-center/app.py](../../../projects/DuberyMNL/command-center/app.py) — `/api/crm/summary` + `/api/crm/orders` redirected to `ORDERS_SHEET_ID` with v3 schema mapping; added `_parse_v3_total` + `_v3_row_date` helpers

**Restart required after edit:** both `chatbot` (monitor restarts the server) and `command-center` (manual `boot-bg.bat` restart) to drop the in-memory old code. /mark-sale and CC CRM tab will keep writing/reading the old sheet until restart.

**Verification after restart:**
- CC CRM tab summary should show 5 orders / P4,339 / 0 today (until a new close hits).
- A test mark-sale via /conversations dashboard should land a row in v3 Orders sheet with `chatbot_mark_sale` in Ad ID col.

**Future iterations to consider:**
- Pull payment method out of Notes into its own column (would require Apps Script update + v3 form update).
- Add `lead_id` column to v3 sheet for explicit chatbot conversation linkage.
- Unified delivery-status reconciler that watches col K and posts back to CRM Leads status.

**Update 2026-05-25:** CC reads side completed via [[cc-dashboard-overhaul-2026-05-25]]. `/api/crm/summary` + `/api/crm/orders` now read from v3 Orders sheet too. Added rolling 24h `orders_today` and 30d `units_sold_30d` metrics. Revenue excludes CANCELED rows. CRM tab redrawn with 5 tiles + click-detail modal.
