---
name: Reference Angle Variety
description: Product refs cause same-angle repetition -- vary angles across batches, don't always use -1.png
type: feedback
related: [feedback_brand_skill_rewrite.md, feedback_creative_range.md, reference_product_refs.md]
originSessionId: 8ef4754d-4499-4e96-b43a-75efcd7fae2c
---
Generated images always face the same angle because we always pass the same reference photo (-1.png / 3/4 front).

**Why:** Content looks repetitive when every image uses the same product angle. Feed variety matters.

**How to apply:** When generating batches, rotate through available SINGLE-VIEW angles (-1, -3, -4) instead of defaulting to -1.png. Mix angles across a batch. **BANNED for generation:** `-2` (multi-angle strip) and `-multi` (composite) -- these confuse Gemini. See `feedback_angle_ban.md`.
