---
name: CSS display overrides [hidden] attribute
description: Elements with explicit CSS display:grid/flex ignore the HTML hidden attribute -- must add [selector][hidden] { display:none !important }
type: feedback
related: [project_v3_order_redesign.md, project_dubery_v3_landing.md]
originSessionId: 1b5360c5-48d1-48bd-a7d7-69594b1ba550
---
CSS `display: grid` or `display: flex` on an element overrides the UA stylesheet's `display: none` that the `[hidden]` HTML attribute normally produces. The element stays visible even when `el.hidden = true` is set in JS.

**Why:** Hit twice in v3 order page — `.order-series-grid { display:grid }` made Bandits/Outback/Rasta accordion dropdowns stay open; `.order-total-row { display:flex }` made the bundle discount row stay visible. Both needed JS `el.hidden = true` to work.

**How to apply:** Any time you add `display: grid/flex/block` to a CSS rule on an element that also uses the `hidden` attribute for JS toggling, add a companion rule:
```css
.selector[hidden] { display: none !important; }
```
Or use a class toggle (`.is-hidden { display: none !important }`) instead of the `hidden` attribute entirely.

**Variant — duplicate selector cascade (session 150):** Two separate CSS blocks for `.lightbox`: first sets `display: none`, second sets `display: flex`. Both have 0-1-0 specificity — the later one wins, making the element always visible. Fix: remove `display` from the second block; add `.lightbox:not([hidden]) { display: flex }` with 0-2-0 specificity so it only fires when the element is not hidden.
