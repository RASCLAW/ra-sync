---
name: DuberyMNL Visual Editor
description: editor.js -- direct manipulation website editor at ?edit, drag/resize/sketch/add/export. Built session 132.
type: reference
related:
  - project_dubery_landing_v2.md
  - feedback_transforms_break_clicks.md
originSessionId: 0aaab2d6-0644-4d3d-bb2f-8639ee9a9d5e
---
**Location:** `dubery-landing-v2/editor.js`
**Activate:** Add `?edit` to any page URL (e.g. `http://127.0.0.1:8124?edit`)
**Zero production impact:** Only loads when `?edit` is in the URL.

**Features:**
- Click = select (red outline + 8 handles), Ctrl+Click = multi-select
- Drag element body = move, drag corner handles = free resize, edge handles = single-axis resize
- Shift+corner = proportional resize
- Text resize changes WIDTH (text wrapping), not font-size
- Double-click = inline text editing (green outline, Enter/Escape to exit)
- Color pickers (text + background) in bottom toolbar
- Sketch/pen tool with canvas undo
- +Text / +Image buttons insert into DOM flow after selected element
- Delete/Backspace hides element, Ctrl+Z undoes everything
- Arrow keys nudge (Shift = 20px jumps)
- E toggles toolbar visibility

**Export format:** Copies to clipboard. Includes:
- Per-element comments: filename, status (VISIBLE/DELETED), parent, position (x,y), size
- CSS changes by selector

**Known limitations:**
- Server (python http.server) dies between file edits -- needs manual restart
- Generic selectors (`img`, `.section-body`) merge changes across elements -- per-element tracking only works for `.ve-added` elements
- Container elements (section, nav, header, multi-child divs) are excluded from selection
- Large transform exports must be converted to layout (padding/margin) before applying -- see `feedback_transforms_break_clicks.md`
