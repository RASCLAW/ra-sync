---
name: cc-sidebar-collapse
description: "2026-05-21 -- CC sidebar collapsible via chevron button; state persists across reloads via localStorage. Works on every tab. Shipped session 166."
metadata:
  node_type: memory
  type: project
  related:
    - reference_cc_command_center.md
    - project_feed_scheduler.md
---

## What
Sidebar in `https://cc.duberymnl.com/` is now collapsible. Click the `‹` chevron on the right edge of the sidebar → shrinks to 64px icon-only strip. Chevron flips to `›`. Click again → expands back to 220px.

## Implementation
- `command-center/templates/shell.html`: button `#sidebarToggle` placed absolute on sidebar's right edge
- `command-center/static/js/shell.js`: `toggleSidebar()` toggles `.collapsed` class on `.sidebar` + persists to `localStorage.getItem("cc.sidebar.collapsed")`. Restores on page load.
- `command-center/static/css/main.css`: `.sidebar.collapsed` rules use **font-size:0 trick** to hide nav-item label text without editing every span. SVG icons keep their fixed width because `.nav-icon` has explicit `width: 18px; height: 18px`. Brand text + agent dot hidden via display:none on child selectors.

**Why font-size:0:** existing nav-items have raw text nodes ("Home", "Monitor", etc.) not wrapped in spans. Wrapping every one in `<span class="nav-label">` would touch ~18 nodes. font-size:0 on parent + explicit SVG width works without template edits.

## How to apply
- Works on every tab automatically (sidebar lives in shell.html)
- localStorage key: `cc.sidebar.collapsed` (value: `"1"` collapsed, `"0"` expanded)
- If sidebar breaks visually on a future tab redesign, check that any new nav-item icons have explicit `width`/`height` on the SVG element

## Related
- [[reference_cc_command_center]] -- CC base URL + auth
- [[project_feed_scheduler]] -- shipped same session
