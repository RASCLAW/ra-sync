---
name: PNG Alphabet → WOFF2 Font Pipeline
description: How to turn a PNG alphabet sample (logo sheet with A-Z glyphs) into a usable web font via Calligraphr + fontTools, no hand-tracing
type: feedback
related:
  - reference_dubery_font.md
  - project_dubery_landing_v2.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
When a brand ships a design-time PNG with an A-Z alphabet sample, we can generate a real TTF/WOFF2 in ~45 min with no SVG tracing. Full pipeline used in session 129 on `DUBERY-FONTS.png`.

**Why it works:** Calligraphr auto-traces each cell of its template into a glyph; fontTools converts the TTF output to WOFF2. The hard part is extracting clean individual letters from a source PNG and pasting them into Calligraphr's grid.

**How to apply — pipeline steps:**

1. **RA creates a Calligraphr account** (free, 75 chars is enough for A-Z).
2. **RA creates a template** in Calligraphr → "English - Uppercase only" (or similar 26-char set) → downloads the blank template PDF → uploads to Drive with "Anyone with link" sharing → shares the URL.
3. **Claude downloads the blank template** via `gdown` from the Drive URL.
4. **Claude de-skews the source PNG** if letters are italic — italic letters touch at baselines and can't be auto-segmented by column gaps. Use a PIL affine inverse shear (`matrix = (1, -s, s*H, 0, 1, 0)` where `s = tan(angle_deg)`) to un-slant before segmentation.
5. **Claude segments 26 letters** from the de-skewed PNG:
   - Find row bands via `row_sum = (arr > threshold).sum(axis=1)` + gap detection.
   - Skip the wordmark row (e.g. "DUBERY") — it's not A-Z sequential.
   - For each row, find column gaps. Italic residue can cause merged pairs (like F+G); auto-split by finding the widest run and splitting at its internal column minimum.
   - Handle row-4 edge cases if the source has extra/duplicate glyphs (in DUBERY-FONTS.png, row 4 was `XVYZ` with a duplicate V — skip glyph index 1).
   - Save each letter as a tight-cropped black-on-white PNG.
6. **Claude renders the Calligraphr PDF → PNG** at 300 DPI via `pypdfium2` (pymupdf's `_extra` DLL load fails on Windows Python 3.12; pypdfium2 is the reliable fallback).
7. **Claude detects grid cells** on the rendered template by finding strong horizontal + vertical grid lines (`ink.sum(axis=N) > threshold`). For a standard Calligraphr template at 300 DPI (A4 → 2481×3508):
   - Rows: label strips at y=254-304, 561-611, 868-918, 1225-1275. Writing areas 307 tall per row EXCEPT row 3 which is 357 tall (no QR block cut off).
   - Cols: 8 vertical lines at x=177, 413, 650, 886, 1122, 1358, 1594, 1831, 2067 (rows 1-2 only use cols 1-6, QR occupies 7-8).
8. **Claude fills cells**:
   - Tight-crop each letter. Compute common scale: `scale = min(usable_w/max_letter_w, target_cap/max_cap_h)`.
   - Align to a **baseline**, not center. Baseline y = label_strip_bottom + 185 (at 300 DPI). Q gets a descender: `body_h = int(nh * 0.87)` — its tail hangs below baseline naturally.
   - Row-4 nudge: shift row 4 up 56px (~2 guide lines) so all rows share visual position within their cells. (Row 4's cell is placed differently by Calligraphr.)
   - For italic variant, re-slant each letter FIRST with forward shear (`matrix = (1, s, -s*H, 0, 1, 0)` with **positive** s = forward lean at top). Getting the shear sign right took one iteration.
9. **RA uploads filled PNG** to Calligraphr → "Upload Template" with "Automatically clean templates" checked → "Build Font" → downloads TTF + OTF.
10. **Claude converts TTF → WOFF2** via `fontTools` + `brotli`:
    ```python
    from fontTools.ttLib import TTFont
    f = TTFont('Dubery-Regular.ttf'); f.flavor = 'woff2'; f.save('dubery-regular.woff2')
    ```
11. **Claude wires `@font-face`** in project styles.css + a CSS var like `--font-dubery` + switches the relevant headings.

**Gotchas:**
- **gdown + private Drive links:** first attempt fails with "Cannot retrieve public link". Either RA changes sharing to "Anyone with link" (usually easier) or saves file locally and says a path.
- **PyMuPDF DLL issue on Windows Py3.12:** `_extra` DLL load fails. Use `pypdfium2` instead.
- **Italic direction confusion:** `matrix = (1, s, -s*H, 0, 1, 0)` with POSITIVE s = forward (top-right) italic. Negative = backward.
- **Letter alignment eyeball:** after first pass, RA called out row-4 being lower than rows 1-3. Fix was a `shift = -56` in the paste loop for row 4 only. Make the row-4 shift a tunable knob from the start.
- **Don't use visual "center in cell" alignment** — it makes shorter letters sit lower than taller letters. Use baseline alignment (fixed offset below label-strip bottom) so cap-heights always align.
