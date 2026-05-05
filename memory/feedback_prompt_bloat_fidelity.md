---
name: Prompt Bloat Breaks Fidelity
description: Stacking creative typography directives (gradient, accent color, identity line, branding-hide, no-bg logo) on top of 3+ product fidelity blocks eats Gemini's attention budget and causes fidelity drift. Start minimal, add one lever at a time.
type: feedback
related: [project_brand_collection_formula.md, feedback_simple_prompts.md, feedback_color_free_specs.md, feedback_stripped_prompt_fields.md]
originSessionId: 4c8d3a18-7f47-44f5-a26a-1a841fdc1451
---
When building brand collection prompts with multiple products, keep the prompt stripped to the essential blocks: fidelity triad + minimal scene (surface/lighting/arrangement) + single headline + logo. Adding creative layers (font gradients, accent colors, multi-line typography, identity lines, branding-hide directives, no-bg logo overrides) individually works but stacking them eats fidelity budget.

**Why:** Session 128 brand-coll batch B3 demonstrated drift at tasks 009 (5-up cross row) and 011 (UNBOX exploded flat-lay). Each prompt had accumulated: gradient typography + accent color + identity line + branding-hide clause + no-bg logo directive + 5 product fidelity blocks + packaging block. Gemini split attention across all constraints — product fidelity degraded (Rasta Red rendered as Bandits Tortoise frame shape in HC1-3, lens colors drifted away from refs). Stripped-down HC4 rerun with same 3 products + single headline + white text + plain logo passed cleanly.

**How to apply:** For any multi-product brand image, start with the minimal formula validated in `project_brand_collection_formula.md`. Add ONE creative lever at a time across a batch (first image = stripped baseline, second image = add gradient, third = add accent color, etc.). If an image fails fidelity, strip back to the last known-clean layer before iterating. Rule of thumb: more products in frame = fewer typography directives in prompt.
