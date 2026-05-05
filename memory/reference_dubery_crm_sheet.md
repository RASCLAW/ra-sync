---
name: DuberyMNL CRM Sheet
description: Google Sheet for customer/lead/order tracking from Messenger chatbot -- structure, sheet ID, access
type: reference
related: [project_chatbot_live.md, project_chatbot_knowledge_base.md]
---

**Sheet Name:** DuberyMNL CRM
**Sheet ID:** `1wVn9WGdY8pK7c68pZpnNSWoNkhhZvYUywcGqLCqcewA`
**URL:** https://docs.google.com/spreadsheets/d/1wVn9WGdY8pK7c68pZpnNSWoNkhhZvYUywcGqLCqcewA
**Location in Drive:** `DuberyMNL/` folder

**4 tabs:**

1. **Leads** -- one row per sender_id (upserted on every customer message)
   - Columns: Lead ID, Name, Phone, Address, Landmarks, Source, First Contact, Last Contact, Model Interest, Status, Notes
   - Status: Cold / Warm / Hot / Converted / Lost
   - Auto-scored by `infer_status()` in `crm_sync.py` based on extracted signals

2. **Orders** -- one row per completed order (created on `order_complete=true`)
   - Columns: Order ID, Lead ID, Items, Quantity, Total, Discount Code, Payment Method, Delivery Preference, Delivery Time, Order Date, Status
   - Order ID auto-generated: `ORD-{YYYYMMDD-HHMMSS}`

3. **Lead Score Log** -- append-only log of status transitions
   - Columns: Lead ID, Timestamp, Previous Status, New Status, Trigger

4. **Conversations** -- every message from every customer (for cold-start recovery + analytics)
   - Columns: Lead ID, Timestamp, Role, Content, Intent
   - Added session 98, not part of original spec

**Access:**
- Shared with Cloud Run service account `371181189379-compute@developer.gserviceaccount.com` as Editor
- Chatbot uses Application Default Credentials (ADC) via `google.auth.default()` in `crm_sync.py`
- Sheets API enabled on `dubery` GCP project

**Code:**
- `cloud-run/crm_sync.py` -- all sync functions (upsert_lead, create_order, append_message, load_history, infer_status, log_status_change)
- Wired into `cloud-run/messenger_webhook.py::process_message()` -- runs inline after bot reply sent
- Errors swallowed: CRM sync failures never block customer replies

**Cold-start recovery:**
- On first message from a sender with empty in-memory store, `process_message` calls `crm_load_history()` to fetch last 20 messages from Conversations tab
- Loads them into ConversationStore so Gemini has full context
- Lets customers return days later and get continuity even across Cloud Run restarts

**Current production data (as of session 106, 2026-04-12):** 25 leads, 0 orders, 27 score log entries, 94 conversation messages. All from the session 97-98 live run before the chatbot went down. Preserved as case-study material for RAS Creative SOLUTIONS.

**Test data cleanup:** `tools/chatbot/cleanup_crm_test_data.py` scans all 4 tabs for rows where the lead identifier starts with `TEST_` (chatbot `/chat-test` endpoint enforces this prefix). `--dry-run` is default, `--confirm` actually deletes. Uses sorted-descending `deleteDimension` batchUpdate so row indices don't shift mid-delete. **Auth: token.json OAuth2 (not ADC)** -- ADC is missing the spreadsheets scope on RA's Windows PC and touching global ADC would affect Vertex AI + Veo tools. Session 106 first run: deleted 61 test rows, preserved 146 production rows.

**Next phase:** Supabase migration planned as portfolio piece. CRM structure will map 1:1 to Postgres tables.
