---
name: Dubery Custom Font (quality-limited, not yet production-ready)
description: Custom web font built from DUBERY-FONTS.png via Calligraphr. Regular-only TTF currently at dubery-landing-v2/assets/fonts/. Rough auto-traced curves -- RA flagged as "not professional enough"
type: reference
related:
  - project_dubery_landing_v2.md
  - feedback_font_build_pipeline.md
originSessionId: 9ed2d7c0-943b-42c1-8b45-12665c1bc67b
---
Built 2026-04-18 session 129 from `dubery-landing/assets/fonts/DUBERY-FONTS.png` (original DUBERY wordmark + A-Z sample sheet). Lives at `dubery-landing-v2/assets/fonts/`.

**Current files (as of 2026-04-18):**
- `Dubery-Regular.ttf` — latest rebuild (uniform 113px cap-height, baseline-aligned to Calligraphr guide lines). 7.4KB. Forward italic slant baked in (~13°).
- `calligraphr-regular.png` / `calligraphr-italic.png` — filled Calligraphr templates (source of truth if regenerating).
- `calligraphr-template.pdf` — blank Calligraphr template (preserves this font's QR code, do not regenerate without a fresh QR).

**Deleted during iteration (no longer present):**
- All `.otf`, `.woff2` files — were generated but deleted when RA asked to retry the build with better alignment.
- `DuberyItalic-Regular.*` — italic variant was built but not kept (RA decided upright letters work with CSS skew or the font's baked-in slant).

**NOT currently wired into styles.css.** The earlier `@font-face` block pointing to `dubery-regular.woff2` / `dubery-italic.woff2` still lives in `styles.css` but references files that were deleted — site now falls back silently to Space Grotesk. Either restore the WOFF2s or remove the dead `@font-face` block next time we touch that file.

**Current status: NOT production-ready.**
RA reviewed the generated font via a 100-word test PDF (lives at `.tmp/font-build/dubery-font-test.pdf`) and called it "not that professional" — Calligraphr's raster-trace leaves wobbly Bezier curves that look amateur next to commercial faces. Options discussed:
- **A)** Swap for Google Fonts lookalike (Rajdhani Bold Italic or Barlow Condensed Black Italic) + use original DUBERY-FONTS.png crop as a **logo image** (not text) for the wordmark specifically.
- **B)** Manual vectorization in Inkscape → FontForge (2-4 hrs).
- **C)** Outsource to Fiverr (~$20-50, 1-2 day turnaround).

Until RA picks a path, the site should stay on Space Grotesk for everything, and the wordmark should be rendered as an image if it needs the exact DUBERY letterforms.

**Font characteristics (if we do keep using it somewhere):**
- 26 uppercase letters only. No lowercase, no digits, no punctuation, narrow space glyph. Any non-A-Z falls back to Space Grotesk via the font-family chain.
- Calligraphr-default spacing is tight (`SHOPNOW` instead of `SHOP NOW`). Fix with CSS `word-spacing: 0.3em` on any element using Dubery.
- Display face only — no hinting, looks rough at body sizes.
- Letters baseline-aligned at Calligraphr guide lines (cap-line = label_bot+39, baseline = label_bot+220, 181px between).

**How to regenerate:** `feedback_font_build_pipeline.md` has the full PNG → WOFF2 pipeline.
