---
name: Veo Prompting Rules
description: Motion-only prompts, negative prompts as nouns, duration constraints, enhance_prompt can't be disabled
type: feedback
related: [project_veo_video_gen.md, reference_vertex_ai.md]
---

Veo video gen prompting rules learned from testing + Google docs:

**Prompt for motion ONLY.** The starting image already provides the scene -- don't re-describe objects. Just describe how they move.

**Negative prompts as nouns.** "morphing, dissolving, duplicating objects" not "don't make things disappear."

**Can't disable enhance_prompt.** Veo 3.1 returns error: "Veo 3 prompt enhancement cannot be disabled."

**Duration must be 4, 6, or 8 seconds** for image-to-video mode. 4s = less hallucination.

**Camera motion is most reliable.** Complex multi-object assembly causes disappearing/duplicating pieces. Simpler motion = better results.

**last_frame param exists!** `GenerateVideosConfig(last_frame=Image(...))` enables start+end frame interpolation. Was undocumented in our memory but confirmed working.

**Seed param available** for reproducibility across multiple scenes.

**Why:** Spent ~$8 iterating on assembly video. Overloaded prompts and complex object motion caused hallucinations (pieces disappearing, multiplying). Motion-only + negative prompt + full model produced acceptable results.

**How to apply:** All future Veo calls should use motion-only prompts. For product videos, prefer camera motion over object assembly. Always include negative prompt for object consistency.
