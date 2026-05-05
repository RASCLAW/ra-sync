---
name: v3 Order Picker State
description: Order form picker port from v1 to v3 -- native select coded, needs custom thumbnail dropdown to match v1
type: project
related: [project_dubery_v3_landing.md, reference_v3_tunnel.md]
originSessionId: 403031e3-ac94-4d66-aeae-dc6953510459
---
v3 /order/ picker rows are coded (HTML + JS + CSS) but use a native `<select>` which doesn't visually match v1's custom thumbnail dropdown.

**Why:** Chose native select for speed; RA reviewed at v3.duberymnl.com and flagged it as "far from v1."

**How to apply:** Next session, port v1's `buildSelect()` custom dropdown (~L460-538 in `dubery-landing/script.js`) into `order.js`. V1 shows product image + name as dropdown items, has compact mode after selection, and excludes already-picked slugs from other rows.

**Files changed this session:**
- `dubery-landing-v3/order/index.html` — card grid replaced with `.picker-rows` container
- `dubery-landing-v3/order/order.js` — full rewrite with row-based picker
- `dubery-landing-v3/styles.css` — picker-row + stepper CSS at L1331+
