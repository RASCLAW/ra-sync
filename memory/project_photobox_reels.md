---
name: PHOTOBOX Reels Pipeline
description: Hyperframes-based FB Reels workflow for PHOTOBOX; Reel #1 keira-birthday RENDERED (21.5MB, 35s); 5-clip montage structure
type: project
related: reference_hyperframes_setup.md, project_photobox_pitch_video.md
originSessionId: 22332c6a-169b-4e44-8856-261e39851d71
---
# PHOTOBOX Promotional Reels

**Why:** Repurpose existing event footage into scroll-stopping FB Reels to promote PHOTOBOX services (kids birthdays, debuts, weddings, corporate) with no new shoots needed.

**How to apply:** When returning to PHOTOBOX reels work, check rendered output first. Next reels use stills from `website/public/photos/events/`.

## Reel Structure (validated, session 4)
```
0-4s     Hook — full-bleed video, big pink hook text slam, cake blow moment (t=209s)
4-21.5s  Montage — 5 clips × 3.5s rapid-cut with white flash transitions + kinetic text overlays
21.5-22.5s  Fade-to-black transition
22.5-27s  Brand close — PHOTOBOX wordmark + tagline over live video, localized panel (78% opacity)
27-28.5s  Video plays clean (overlay fades out)
28.5-33s  BOOK NOW pill CTA + sub-text
33-35s   Fade-to-black
```
Key lessons:
- Video must play for ALL 35s — no solid bg scenes
- `object-fit: cover` on `.vid` (width 1080px, height 1560px, top 180px) shows ~40% more horizontal frame
- Videos need z-index 2 or lower; text overlay divs need higher z-index (text otherwise invisible)
- Cinematic filter: `brightness(0.82) contrast(1.14) saturate(1.3)` on all `.vid` elements
- All video/audio elements need `id=` attributes or Hyperframes throws `media_missing_id` lint error
- Source video has 5s sparse keyframes — can cause frame freezing; re-encode with ffmpeg if needed

## Files
- `C:\Users\RAS\projects\PHOTOBOX\reels\keira-birthday\` — Reel #1 (Kids Birthday, 35s, 1080×1920)
  - `index.html` — Hyperframes composition (linted clean, 0 errors)
  - `video.mp4` — Kiera7th.mp4 copy (1280×720, 250s, Ellisha Keira 7th birthday)
  - `DESIGN.md` — PHOTOBOX brand system for reels
  - `renders/keira-birthday_2026-05-03_07-21-58.mp4` — **RENDERED** (21.5MB, 35s)

## Brand Colors (confirmed from Final Brand System HTML)
- Primary teal: `#0d9488` | Dark teal: `#0f766e` | Deep bg: `#0d2b2b`
- Birthday accent: `#ec4899` pink | Debut accent: TBD | Corporate: TBD
- Text: `#ffffff` primary, `#d4f0e8` mint secondary

## Key Technical Notes
- Render command: `PRODUCER_HEADLESS_SHELL_PATH="C:\Users\RAS\AppData\Local\ms-playwright\chromium-1217\chrome-win64\chrome.exe" npx hyperframes render`
- Transcript timestamps (Gemini-generated) are real video positions — no speed correction needed
- Always lint before render: `npx hyperframes lint` — 0 errors required; warnings OK
- `<audio src="video.mp4">` plays in browser preview but renders SILENT — Hyperframes audio pipeline needs a pure audio file (`.mp3`/`.aac`). Extract first: `ffmpeg -i video.mp4 -vn -acodec libmp3lame -q:a 2 audio.mp3`
- One continuous audio element beats per-clip audio elements — avoids volume jumps and seeking quirks between cuts

## Next Reels
- Debut (use `website/public/photos/events/debut/` stills, accent color TBD)
- Corporate (use `website/public/photos/events/corporate/` stills)
- Wedding (use `website/public/photos/events/wedding/` stills)
