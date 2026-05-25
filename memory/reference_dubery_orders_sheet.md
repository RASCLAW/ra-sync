---
name: DuberyMNL Orders Sheet + Apps Script Webhook
description: Orders sheet id + Apps Script webhook URL + the FormData payload shape that actually works (raw JSON body silently fails)
type: reference
related:
  - project_dubery_v3_landing.md
  - reference_dubery_crm_sheet.md
  - project_order_tg_recovery.md
  - reference_googleapi_httplib2_fallback.md
originSessionId: 70bad458-80be-401f-b689-c0aedf9cdc10
---
**Sheet:** "DuberyMNL Orders" — id `1vS-yuFWovqHYWrFte4QXJLtH3Q2-BRDi6i9P4-vXbkA`
**Webhook:** `https://script.google.com/macros/s/AKfycbxFD6z-PR8tcQhHpH-UulU-3Nk8OpqWgWeyD-J0unJser7Cptd4tP-3D6iM8W0eOoWCtg/exec`

**As of 2026-05-24 this is the SINGLE source of truth for closed sales.** The chatbot's `/mark-sale` endpoint (via `crm_sync.create_order`) writes directly to this sheet using the same schema. Ad ID column tags origin: `order_form` = v3 PDP webform; `chatbot_mark_sale` = manual close from /conversations dashboard. CRM > Orders tab in the CRM sheet is deprecated. See [[orders-consolidation-2026-05-24]].

**Payload shape that works:**

```js
const payload = {
  name, phone, address, notes,
  items: [{ name: "Bandits – Tortoise", qty: 1 }, ...],  // EN-DASH separator
  caption_id: "order_form" | `pdp_${slug}`,
  grand_total: 698,   // number in PHP
  delivery_fee: 99,   // 99 single, 0 on 2+ pairs
  express: false,
};
const fd = new FormData();
fd.append("payload", JSON.stringify(payload));
await fetch(APPS_SCRIPT_URL, { method: "POST", mode: "no-cors", body: fd });
```

**Why this exact shape:** Apps Script reads the body via `e.parameter.payload`. If you POST raw JSON as the body (no FormData wrapper), `e.parameter.payload` is empty and the order silently drops — NO error surfaces because `mode: 'no-cors'` means you can't read the response.

**Item name format:** `"{Series} – {Colorway}"` with EN-DASH (U+2013, not ASCII hyphen). Stored in v3 `data.json` as `order_name` field per variant. v1 sheet convention — keep it.

**How to apply:** Any new frontend POST from duberymnl.com (PDP, /order/, future cart, chatbot fallback) must use FormData+payload wrapper. Test with a smoke order before going live; check sheet row landed with correct item names and totals.

---

## TG order pings (live as of 2026-05-19)

`doPost` calls `notifyTelegram(data)` after `saveToSheet`. Function lives in the same Apps Script, uses **hardcoded** token + chatId (NOT Script Properties — two unused property rows exist named `TELEGRAM_BOT_TOKEN` + `TG_CHAT_ID`, ignore them or wire them up later).

**Gotchas that burned us once:**
- **Deployment version pinning.** Editing the script does NOT update the live webhook URL. You must `Deploy → Manage deployments → ✏️ pencil → Version: New version → Deploy` for code changes to go live. URL stays the same across new versions. Symptom of stale deploy: Executions log shows `doPost` runs as "Completed" but no TG pings fire — the old version is running.
- **Outer try/catch swallows TG errors.** `doPost` wraps everything in `try { ... } catch (err) { Logger.log(...) }`, so a 403 from Telegram (bot not started by recipient) or wrong chat ID looks identical to success in the Executions list. To debug, click the execution row and read `Logger.log` output.
- **Bot must be /start-ed by recipient.** Telegram blocks bots from initiating DMs. The chat owner must send at least one message to the bot first.
- **Smoke test before redeploy:** add a `testTg()` function that calls `notifyTelegram({name,phone,address,items,grand_total})` with dummy data. Select `testTg` in the function dropdown (NOT `doPost` — that needs `e.parameter`) and ▶ Run. Phone buzz = good.

**Root-cause incident (weekend of 2026-05-14 → 16):** 5 orders landed in the sheet with zero TG pings. Cause: deployed Version 6 predated the `notifyTelegram` wiring. Fix was a one-click "New version" redeploy.

---

## Column conventions beyond the headers (sheet writers must know)

The header row only declares 10 columns (Timestamp / Name / Phone / Address / Items / Qty / Delivery Fee / Total Amount / Notes / Ad ID), but two extra columns are in active use without headers:

- **Col K -- manual status** (no header). RA writes `DELIVERED` when an order has been dispatched / handed to courier; left blank when cancelled. Convention as of 2026-05-20 treats dispatch and customer-receipt as the same state -- there is currently no `OUT_FOR_DELIVERY` vs `DELIVERED` split. If you need to distinguish those, ask RA before introducing new vocab.
- **Col L -- courier pickup timestamp** (no header). Started 2026-05-20 during the weekend-order recovery to add audit-trail evidence without overwriting col K. Format: `Picked up YYYY-MM-DD HH:MM AM/PM (courier from 115 I. Sanchez)`.

**How to apply when writing to this sheet:**
- Never overwrite an existing col K value -- RA's marks are intentional; ambiguity = ask, not stomp.
- Notes (col I) is for customer delivery instructions only -- do not mix in courier/timing metadata there.
- For dispatch evidence (courier handoff, pickup time, lalamove/grab tracking) use col L additively.
- Use [[reference_googleapi_httplib2_fallback]] when `read_sheet.py` times out -- the requests-based fallback is the only path that worked from RA's home network during the recovery session.
