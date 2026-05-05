---
name: data-field Removal Crash
description: Removing a data-field HTML element without cleaning its JS reference causes a null crash that kills all subsequent JS on the page
type: feedback
related: [project_v3_pdp_cart_redesign.md, project_v3_order_enhancements.md, feedback_nested_button_html.md]
originSessionId: 6f096e3d-7dd1-44e9-9e25-e2b60f9781a1
---
Removing an HTML element that has a `data-field` attribute (e.g. `data-field="messenger"`) will crash item.js if the JS still contains `document.querySelector('[data-field="messenger"]').href = ...`. The querySelector returns null, and calling `.href` on null throws a TypeError that stops all subsequent script execution on the page.

**Why:** item.js uses a `set()` helper and direct querySelector calls to hydrate PDP fields. If a field is removed from HTML but not from JS, the crash is silent in the UI — Add to Cart button just stops working, People Also Bought disappears.

**How to apply:** Before removing any HTML element with a `data-field` attribute from a PDP template, grep item.js for that field name and remove the corresponding JS line too. Check for both `set('field-name', ...)` and `document.querySelector('[data-field="field-name"]')` patterns.
