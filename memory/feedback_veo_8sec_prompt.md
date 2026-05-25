---
name: feedback-veo-8sec-prompt
description: Beat-by-beat 8-second prompt structure prevents Veo from improvising after initial motion completes
metadata: 
  node_type: memory
  related: 
    - feedback_veo_prompting.md
    - project_cc_video_tab.md
  type: feedback
  originSessionId: f29ecc51-1ba5-4419-a23a-1a504a4c3a30
---

Always describe the full 8 seconds when prompting Veo for image-to-video. Use "Seconds 1-2: ..., Seconds 2-4: ..., Seconds 4-6: ..., Seconds 6-8: ..." structure.

**Why:** Short prompts like "slow push-in toward the sunglasses" only describe the first 1-2 seconds. Veo fills the rest of the clip with whatever it thinks fits, often drifting away from the product or doing unexpected things. Full timeline prompts keep Veo on task for the entire clip duration.

**How to apply:** Before generating any image-to-video, read the starting frame and write a 4-beat prompt covering the full clip. Structure:
- Seconds 1-2: establish the scene + opening camera move
- Seconds 2-4: primary action / product highlight moment
- Seconds 4-6: continuation / transition
- Seconds 6-8: hold/resolve on final composition

Tested and confirmed across 4 parallel tortoise Bandits shots — all 4 passed with richer motion than single-beat prompts.
