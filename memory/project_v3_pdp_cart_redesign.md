---
name: v3 PDP Cart Redesign
description: PDP no longer has PII form -- Add to Cart + localStorage cart badge; order page is checkout
type: project
related: [project_v3_order_redesign.md, reference_dubery_orders_sheet.md, project_dubery_v3_landing.md, project_v3_order_enhancements.md, feedback_data_field_removal_crash.md]
originSessionId: 4eb6ffb9-b3a9-475e-bde5-b5ff542047ea
---
PDP (`products/item.html`) no longer collects name/phone/address. Replaced with "Add to Cart" button that writes to localStorage and updates a nav badge across all pages.

**Cart schema:** `localStorage.key = 'dubery-cart'`, value = `{ "bandits-matte-black": 1, "outback-black": 2 }` — slug → qty, mirrors `order.js` qtys object directly.

**Flow:**
1. PDP → Add to Cart → badge increments, button flips "Added ✓" and stays (no revert — session 150)
2. User navigates to `/order/` — cart.js reads localStorage, order.js merges into qtys on init
3. User submits order → `localStorage.removeItem('dubery-cart')` → badge resets

**New file:** `dubery-landing-v3/cart.js` — shared `updateCartBadge()`, included on all 4 pages via `<script>`.

**PDP additions:**
- "You might also like" inline strip: 4 random SKU thumbnails, `repeat(4,1fr)` grid, above product name
- Gallery prev/next arrows overlaid on main image + touch swipe (40px threshold)
- Bottom section: "Pick your style" series cards (reused from homepage, not custom grid)

**Why:** Separating browsing (PDP) from checkout (order page) reduces friction and enables multi-item cart.

**How to apply:** When editing order flow — PII lives only in `order/index.html`. PDP is description + cart only. `dubery-cart` localStorage is the cart state handoff between pages.
