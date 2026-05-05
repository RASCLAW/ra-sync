---
name: Dual Monitor Setup
description: RA's dual monitor layout -- big monitor at 0,0 (1920x1080), laptop at 1920,209 (1366x768)
type: reference
related: [reference_ffmpeg.md]
---

RA has a dual monitor setup as of 2026-04-05:
- **Display 2 (big monitor):** 1920x1080 at position (0,0) -- primary work screen
- **Display 1 (laptop):** 1366x768 at position (1920, 209)

For FFmpeg screen recording:
- Big monitor: `-offset_x 0 -offset_y 0 -video_size 1920x1080`
- Laptop: `-offset_x 1920 -offset_y 209 -video_size 1366x768`
- FFmpeg path: `C:/Users/RAS/AppData/Local/Programs/Python/Python312/Lib/site-packages/imageio_ffmpeg/binaries/ffmpeg-win-x86_64-v7.1.exe`
