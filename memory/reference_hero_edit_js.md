---
name: hero-edit.js Visual Crop Editor
description: Floating panel at ?edit URL for dialing in hero image object-position and zoom; outputs Copy CSS values
type: reference
related:
  - project_dubery_v3_landing.md
  - reference_v3_pdp_gallery_editor.md
originSessionId: d9abb3be-74ff-423d-9e09-bde284084605
---
**File:** `dubery-landing-v3/hero-edit.js`
**Activate:** append `?edit` to any page URL that includes the script tag (currently injected into `index.html`)

**Controls:**
- X Position slider (0–100%) → `object-position X`
- Y Position slider (0–100%) → `object-position Y`
- Zoom slider (0.5x–2.5x) → `transform: scale(N)` anchored to focal point
- Copy CSS button → copies final values to clipboard

**Output format:**
```
object-position: 57% 28%;
transform: scale(1.25);
transform-origin: 57% 28%;
```

Paste values into `styles.css` mobile or desktop block and bump the version tag.

**Note:** Zoom uses `transform: scale()` on the img; parent has `overflow: hidden` so clipping works. Scale < 1 = zoom out (may show background color at edges). Scale > 1 = zoom in (more crop).
