---
name: video-dissection-method-validated
description: "2026-05-19 dissection→recreate test hit ~80% fidelity on first pass; format is actionable, skill promotion unblocked"
metadata: 
  node_type: memory
  related: 
    - reference_video_dissection_workflow.md
    - feedback_video_dissection_consecutive_frames.md
    - project_dubery_trailer_v1.md
    - feedback_no_skeleton_reskin.md
  type: project
  originSessionId: 4ac03bd6-5087-46e3-92ef-cc91a144e7a7
---

The video dissection workflow (frame-by-frame consecutive reads + transition vocabulary inventory + animation techniques table + story arc) was validated 2026-05-19 by reproducing the ElevenLabs Scribe v2 Realtime trailer at ~80% fidelity from the dissection doc alone — RA's verdict after seeing it run on studio.duberymnl.com.

**Why:** Phase 2 of the handoff (promote `/dissect-video` to a proper skill) was gated on this test. The premise was: if the dissection format gave enough detail to recreate the original, the format is good enough to ship as a skill. 80% on a first-pass blind recreate clears that bar.

**How to apply:**
- Phase 2 (build `/dissect-video` skill) is unblocked. Spec is in [[reference_video_dissection_workflow.md]].
- Recreate artifact: `~/projects/hyperframes/elevenlabs-scribe-recreate-v1/index.html` — 568 lines, Hyperframes + GSAP, 1080×1920, 30s, all 12 scenes from the dissection.
- Key proof: the 3-mechanic transition vocabulary (whip-pan left, white flash, hard cut) translated cleanly from prose description to GSAP timeline. That's the hardest part of any motion-graphics replication — if the dissection captures *that*, everything else follows.
- Source dissection: `~/projects/DuberyMNL/.tmp/HnVideoEditor_2026_05_19_231144031_dissection.md`.
- The missing 20% is mostly: faithful "II" glyph (used Unicode `‖`), iPhone 3D render quality (CSS rounded rect approximation), iridescent gradient richness on the CoverFlow cards, and exact font-pair match (Inter + JetBrains Mono close approximations).

Do NOT promote the skill before running the validation pass: 1 longer video (2-3 min, tests chunking) and 1 live-action video (tests format adaptation beyond motion graphics).
