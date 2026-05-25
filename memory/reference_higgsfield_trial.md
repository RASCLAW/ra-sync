---
name: reference-higgsfield-trial
description: "Higgsfield free trial details, pricing, and Claude Code install path for video generation testing"
metadata: 
  node_type: memory
  type: reference
  related: 
    - project_veo_video_gen.md
    - project_cc_video_tab.md
  originSessionId: 2d5a7d12-3905-4ec6-8492-76c15a615d01
---

# Higgsfield AI — Trial & Install Reference

## Pricing (verified 2026-05-17)

| Plan | Price | Credits/mo | Resolution | Notes |
|---|---|---|---|---|
| Free | $0 | 50/mo | 720p max | Watermarked, no commercial license, 8s max clip |
| Creator | $29/mo ($24 annual) | 500 | 1080p | Full features, commercial license, no watermark |
| Studio | $199/mo ($166 annual) | 5,000 | 1080p | API access, teams |

Note: Jono Catliff cited $15/mo in April 2026 — this was the old Starter price, now replaced by Creator at $29/mo.

## Free Tier Reality

- 50 credits/mo at signup — no timed trial
- Watermarked, 720p max, 8s max clip
- MCP uses same credit pool (works on free plan)
- No credit giveaways built in — Higgsfield runs random giveaways on X only
- 50 credits ≈ 3-4 short test videos at low res

## Install Path (Claude Code)

```
/plugin marketplace add higgsfield-ai/skills
```

Four skills installed:
- `/higgsfield:generate` — 30+ models + Marketing Studio + Virality Predictor
- `/higgsfield:product-photoshoot` — 10 modes (studio, lifestyle, Pinterest, hero banner, etc.)
- `/higgsfield:soul-id` — reusable face-faithful identity anchor
- `/higgsfield:marketplace-cards` — product card generation

No manual MCP wiring needed. Auth via Higgsfield account login.

## Marketing Studio Video Modes

All produce up to 15s, 9:16 social clips from a product URL or image:

- `ugc` — phone-shot organic content feel
- `ugc_unboxing` — unboxing reveal
- `ugc_how_to` — tutorial format
- `product_review` — presenter-style review
- `product_showcase` — clean product feature
- `tv_spot` — broadcast-quality ad
- `ugc_virtual_try_on` — virtual try-on
- `wild_card` — unconstrained creative

## Test Plan for DuberyMNL

1. Sign up free at higgsfield.ai
2. `/plugin marketplace add higgsfield-ai/skills`
3. Drop a Bandits or Outback product ref image
4. Test `ugc` + `tv_spot` modes first (highest contrast in style)
5. If quality clears for FB → upgrade to Creator ($29/mo)

**How to apply:** Read this before the Higgsfield trial session. Sequence: Seedance kie.ai test first (zero cost), then Higgsfield free trial.
