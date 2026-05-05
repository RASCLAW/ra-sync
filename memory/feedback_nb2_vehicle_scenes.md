---
name: NB2 Vehicle Scene Limitations
description: NB2 struggles with car/motorcycle interiors -- spatial errors on mirrors, dashboards, traffic flow. Avoid or simplify these scenarios.
type: feedback
related: [feedback_naturalism_prompting.md, feedback_simple_prompts.md]
---

CAR_SELFIE and motorcycle scenarios produce spatial reasoning errors in NB2 -- side mirrors in wrong positions, traffic not matching across windows, dashboard layout inconsistencies.

**Why:** AI image gen can't handle vehicle interior spatial consistency. RA noticed this across multiple generations.

**How to apply:** CAR_SELFIE and motorcycle scenarios are now REMOVED from the UGC pipeline scenario library entirely (confirmed session 87). Do not generate these. For DASHBOARD_FLEX (product on dashboard, no person), keep it simple -- the product is the subject, not the vehicle interior.
