---
name: Randomizer Multi-Product Mode
description: Omit --product flag in v3_randomizer.py to get a mixed-product batch with no-repeat dedup
type: reference
related: [reference_pipeline_skill_chain.md, reference_ugc_pipeline_skill.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
`tools/image_gen/v3_randomizer.py` now supports multi-product batches.

**Behavior:**
- `--product <key>` given → whole batch locked to that product
- `--product` omitted → randomizer picks a different product per image (no-repeat until all 11 products exhausted, then reshuffles)
- Same no-repeat logic also applies to category within a batch

**Skill invocation patterns:**
- `ugc-pipeline bandits-blue 3` → single-product batch
- `ugc-pipeline 10` → mixed batch (10 images across different products)
- `ugc-pipeline bandits-tortoise UGC_SELFIE 3` → locked product + category

**Why:** Session 122 -- RA asked "can i say run ugc pipeline 10?" to trigger a mixed batch without typing a product key. Added `batch_products: set` threading through `randomize_one()`, mirroring the existing `batch_categories` no-repeat logic.
