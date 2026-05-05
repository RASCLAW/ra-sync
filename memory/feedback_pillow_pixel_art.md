---
name: Pillow Pixel Art Preference
description: When generating visuals with Pillow, lean into pixel art (blocky sprites, scaled-up grids) not smooth vector shapes (ellipses, arcs). Pillow can't do smooth cartoon characters well.
type: feedback
related: [reference_demo_video_pipeline.md]
---

Pillow is good at pixel art, not smooth vector-style characters. When RA asked for Haminations-style (smooth round heads, curves), the results looked "low effort." Switching to pixel art sprites (16x16 grids scaled up) produced much better results.

**Why:** Pillow's drawing primitives (rectangles, ellipses) can't produce smooth anti-aliased cartoon characters at the quality RA expects. Pixel art is intentionally blocky, so it looks deliberate, not broken.

**How to apply:** For any Pillow-based visual generation, default to pixel art / retro game aesthetic. If smooth cartoon visuals are needed, research alternative tools (Blender, Mine-imator, HTML/CSS + Playwright screenshot) instead of forcing Pillow.
