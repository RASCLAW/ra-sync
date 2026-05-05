---
name: Blob URL Not Base64 for Browser File Uploads
description: Use blob URLs + data-uploadedFilename for browser file pickers in editors; base64 in JSON causes massive file bloat
type: feedback
related: [reference_v3_pdp_gallery_editor.md, reference_catalog_hover_workflow.md]
originSessionId: bc5e6338-53fa-448a-99ae-c946ebdd0c60
---
Use `URL.createObjectURL(file)` for preview and store `file.name` as `img.dataset.uploadedFilename`. When saving to JSON, write `../assets/catalog/<filename>` instead of the base64 data URL.

**Why:** Base64-embedding uploaded images in JSON creates 5–6MB files for just 3 images. The first two data.json drops this session were that large. Switching to blob URLs + filename keeps the JSON small and clean.

**How to apply:** Any time building a browser-side image editor that saves to a JSON config file. Always:
1. `img.src = URL.createObjectURL(file)` — for preview
2. `img.dataset.uploadedFilename = file.name` — for path reconstruction
3. On save: `../assets/catalog/${img.dataset.uploadedFilename}` — for the JSON path
4. Tell RA to copy the uploaded files to `assets/catalog/` to complete the save
