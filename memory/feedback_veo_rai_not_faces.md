---
name: feedback-veo-rai-not-faces
description: Veo RAI filter does NOT blanket-block faces in image-to-video; failure was image/prompt-specific
metadata: 
  node_type: memory
  related: 
    - project_cc_video_tab.md
    - feedback_veo_prompting.md
    - feedback_veo_rai_composition.md
  type: feedback
  originSessionId: f29ecc51-1ba5-4419-a23a-1a504a4c3a30
---

Do not tell RA that Veo blocks faces in image-to-video -- it does not.

**Why:** Session 157 tested two image-to-video calls with AI-generated UGC person shots (faces clearly visible). Both succeeded (2.2–2.3MB, lite, 9:16). The 22:53 failure that prompted this investigation returned `rai_media_filtered_count=1` for an unknown reason specific to that image/prompt combination. The agent's conclusion ("no faces") was wrong.

**How to apply:** If Veo RAI filter fires (`rai_media_filtered_count=1`), try a different prompt or retry before assuming the image content is the cause. Faces in AI-generated images are not a reliable trigger. Real person photos (non-synthetic) may behave differently -- untested.
