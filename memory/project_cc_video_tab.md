---
name: project_cc_video_tab
description: Command Center Video tab -- Veo 3.1 generation UI, current state, known quirks
metadata:
  type: project
related: feedback_flask_template_cache, feedback_cc_kill_reliable
---

# CC Video Tab

## Status
Live in Command Center (`cc.duberymnl.com`, port 8090). First video generated successfully (21:22, `video_1778937670574.mp4`, 2.2MB, lite model). UX iteration complete as of session 157 savepoint 2.

## What's built
- Model pills (fast/full/lite), aspect ratio pills (9:16/1:1/16:9), audio toggle
- Paste image into direction box → sets as starting frame; thumbnail shows below chat thread; absorbed into user bubble on Ask
- Preset chip: "Use attached image" with product fidelity instructions baked in
- Ask = chat thread (confirm interpretation, no tools); Generate = SSE stream + cost confirm + elapsed timer
- Progress bar: pulsing dot + elapsed timer with milestone labels (Writing prompt → Submitting → Generating...)
- `/api/video-bank` endpoint: scans `contents/new/*.mp4` + sidecar JSON, returns metadata
- Video bank panel (bottom right): compact 80×56px rows, play/pause toggle, loads on tab open, appends on new generation
- Path normalization: `--image` path forward-slashed before agent sees it, no quotes

## Pipeline
Direct: agent writes Veo motion prompt → runs `tools/image_gen/generate_videos.py` → Veo 3.1 Lite/Fast/Full via Vertex AI (`dubery` project, `us-central1`). NOT through the image pipeline (no Sheets, no kie.ai, no validator).

## Key flags
- `--image` = starting frame (image-to-video anchor)
- `--ref-image` = product fidelity reference (not in UI yet -- backlogged)
- `--last-frame` = end frame for interpolation (not in UI yet -- backlogged)

## Known quirks
- Flask debug=False: template changes need CC restart; JS/CSS served fresh
- Windows paths: always normalize backslashes → forward slashes before injecting into agent command strings
- `upload-concept` multipart fix: endpoint now handles FormData (file) AND base64 JSON
- Veo generation takes 2-5 min -- progress timer fills the silence
- `extractVideo` regex catches both relative (`contents/new/`) and absolute Windows paths

## Costs
- Lite: ~$0.50-1/video
- Fast: ~$1/video
- Full: ~$3-4/video
- GCP credits: $218 remaining, expires 2026-07-05

**Why:** How this shapes suggestions -- use lite for all tests; fast/full only for hero content.
