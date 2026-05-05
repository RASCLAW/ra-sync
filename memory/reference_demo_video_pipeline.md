---
name: Demo Video Pipeline
description: Automated demo video generation -- TTS (edge-tts), screen recording (FFmpeg), animated explainer (Pillow)
type: reference
related: [feedback_pillow_pixel_art.md, project_scroll_site.md, project_video_content_path.md, reference_ffmpeg.md]
originSessionId: b2200ee9-31b1-498c-9630-f1baed7d77c6
---
Prototyped in session 79. Tools:
- **TTS:** edge-tts (free Microsoft voices, `en-US-GuyNeural`, pip install edge-tts)
- **Screen recording:** FFmpeg via imageio_ffmpeg (gdigrab on Windows)
- **Animation:** Pillow frame-by-frame -> FFmpeg compile
- **Video editing:** MoviePy (concatenate, overlay, audio sync)

Scripts at: `~/projects/automation-workflows/make/demo-scripts/`
- `generate_animation.py` -- animated explainer generator
- `capture-lead-router.py` -- Selenium-based screenshot capture
- `lead-router-demo.md` -- narration script

Note: Playwright Python has greenlet DLL issue on Windows. Use Selenium or Playwright MCP instead.

**2026-04-25 update:** Hyperframes + VideoUse is now a stronger path for branded portfolio-quality demo videos. VideoUse auto-trims raw footage; Hyperframes renders HTML → MP4 with motion graphics. Both are free/open-source. See EA-brain summary: `references/summaries/nate-claude-video-editing-v2.md`. Keep edge-tts + Pillow pipeline for quick/cheap output; use Hyperframes for DuberyMNL chatbot demo + RAS Creative portfolio demos.
