---
name: Veo Video Generation
description: Veo 3.1 tested and working -- Fast is default tier, supports speech/audio, product refs via image param
type: project
related: [feedback_veo_prompting.md, project_video_content_path.md, reference_gcloud_cli.md, reference_vertex_ai.md]
---

Veo 3.1 video generation working as of session 90.

**Why:** DuberyMNL needs video content (UGC reels, product reveals, social ads). Veo generates 8s videos with speech for ~$1 each -- cheaper than hiring UGC creators ($50-200/video).

**How to apply:**
- Use Veo 3.1 Fast as default (best quality/cost ratio)
- Veo 3.1 Lite for bulk/testing
- Veo 3.1 Full only for hero content
- Veo 2.0 is dead -- no audio, no speech
- For realism: add "shot on iPhone, real skin texture, no CGI, no 3D rendering" to prompts
- Speech: include dialogue in quotes in the prompt
- Product refs: pass as `image` param (image-to-video) -- untested: `reference_images` with ASSET type for better fidelity
- Prompt language tip: Veo can generate Taglish dialogue
