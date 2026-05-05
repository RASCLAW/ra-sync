---
name: DuberyMNL Orders Sheet + Apps Script Webhook
description: Orders sheet id + Apps Script webhook URL + the FormData payload shape that actually works (raw JSON body silently fails)
type: reference
related:
  - project_dubery_v3_landing.md
  - reference_dubery_crm_sheet.md
originSessionId: 70bad458-80be-401f-b689-c0aedf9cdc10
---
**Sheet:** "DuberyMNL Orders" — id `1vS-yuFWovqHYWrFte4QXJLtH3Q2-BRDi6i9P4-vXbkA`
**Webhook:** `https://script.google.com/macros/s/AKfycbxFD6z-PR8tcQhHpH-UulU-3Nk8OpqWgWeyD-J0unJser7Cptd4tP-3D6iM8W0eOoWCtg/exec`

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
