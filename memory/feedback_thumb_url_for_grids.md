---
name: thumb-url-for-grids
description: Any grid of more than a few images must use /api/thumb/<path>?w=N for the src, never /api/images/<path>. 100x perf gap -- full PNGs are ~1.5MB each, thumbs are ~15KB. Lightboxes/details use full URL on click, grids never do.
metadata:
  type: feedback
related:
  - project_image_bank_overhaul
  - project_cc_dashboard_overhaul_2026_05_25
  - reference_cc_command_center
---

**Rule:** In any CC surface that renders multiple images at once (image bank grid, schedule picker, queue card detail modal, content gen results), the `<img src>` MUST point at `/api/thumb/<path>?w=N` -- never the raw `/api/images/<path>`. Reserve full-resolution URLs for single-image lightboxes/previews where the click already happened.

**Why:** Full PNGs in `contents/new/` and `contents/ready/` average ~1.5MB. The `/api/thumb` endpoint serves a cached 240px or 480px JPEG averaging ~15KB. For 4 images: 6MB vs 60KB = ~100x. For 50 images in a grid: 75MB vs 750KB. The cache is disk-backed (`.tmp/thumb_cache/`) and keyed by source mtime, so re-edits regenerate automatically.

**How to apply:**

- **Grids (any tab):** Use `url.replace('/api/images/', '/api/thumb/') + '?w=240'` (or 480 for retina). Allowed widths are `{120, 180, 240, 320, 480, 640}` -- snaps to nearest if you pass an off-list size.
- **Queue card detail modal FB-style preview:** Use `?w=480` because the FB card caps at 560px and cells can hit 280×280 (480 gives retina headroom).
- **Lightbox / single-image expansion:** Use the full `/api/images/<path>` URL because the click is the trigger and only one image loads.
- **Schedule preview composer** (`renderMultiPreview`/`renderCollagePreview`): currently uses full src_url; if RA reports composer slowness with many images selected, switch to thumb URL there too.

**Bug pattern this prevents:** When you add a new image-display surface, default to whatever URL field the API already returns. The image-bank endpoint returns only `url` (full); the schedule-bank endpoint returns both `src_url` (full) AND `thumb_url`. Always pick the thumb. If the surface only has `url`, do the string replace inline.

**Failure mode example:** 2026-05-25 -- the queue card detail modal was loading full PNGs. 4-photo Pixel Perfect Shades post = ~6MB per modal open. Felt extremely slow. Swapped to `?w=480` thumbs -> ~60KB per open, instant. ([[project_cc_dashboard_overhaul_2026_05_25]] for full session context.)

**Safety net:** When using a thumb URL, also wire a one-shot `error` listener that falls back to the full URL:
```js
imgEl.addEventListener('error', function once() {
  imgEl.removeEventListener('error', once);
  imgEl.src = fullUrl;
}, { once: true });
```
This handles the rare case where thumb generation fails (corrupt source, unsupported format).
