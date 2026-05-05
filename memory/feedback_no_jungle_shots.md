---
name: No Jungle Shots
description: RA dislikes dense tropical jungle path scenes — drop from location rotation
type: feedback
related: [feedback_no_night_scenes.md, feedback_narrow_scenarios.md, project_content_skill_iterations.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
Remove or avoid "dense tropical jungle path with dappled sunlight through canopy" (loc#15 in LOCATIONS_PERSON) from the bandits/outback/rasta scene rotation.

**Why:** Session 122 -- RA passed the rasta-red jungle wearing shot but flagged "i dont like jungle shots." Jungle scenes read as generic/overused and don't match the premium beach/urban/countryside direction we want for DuberyMNL content.

**How to apply:**
- Pull loc#15 from `LOCATIONS_PERSON` in `tools/image_gen/v3_randomizer.py`, or mark deprecated
- Bamboo grove (loc#30) is fine -- that one worked for bandits-tortoise
- If a jungle scene lands in future randomizer output, swap to a beach/rooftop/coastline/countryside alternative before generating
