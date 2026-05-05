---
name: Session 99 -- Karpathy refactor recovered + committed as fc3bddf
description: Crashed Karpathy/Nate Herk refactor session recovered from uncommitted state. brand-callout + brand-collection fidelity port still parked pending QA bandwidth.
type: project
related: [feedback_brand_skill_rewrite.md, project_brand_pipeline.md, feedback_content_storage_rule.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
Session 99 (2026-04-10 -> 04-11) recovered the uncommitted work from RA's earlier crashed remote session that was applying Karpathy's LLM wiki pattern + Nate Herk prompting principles to the brand/UGC skills. The work had been sitting on disk at risk of loss for over a week while RA pivoted to the Messenger chatbot push.

**What was recovered (committed as fc3bddf):**
- New canonical content directory: `contents/{new,ready,failed,assets}/`
- All 13 brand/UGC skills updated with new `contents/assets/` paths (fonts, logos, product-refs)
- 8 new `REFERENCES.md` I/O manifests per skill (Karpathy wiki pattern -- lightweight reads/writes/depends-on/referenced-by)
- Tool scripts rewired to new paths: `generate_vertex`, `run_ugc`, `run_wf2`, `status.py`, etc.
- `brand-bold` fully rewritten with WF2 fidelity rules (R2 banned descriptors, R3 render_notes template, R4 lens reflection)
- 80+ scraped supplier reference images in `references/supplier-images/` (gitignored, Drive backup)
- `archives/` directory preserving old `output/images/` files as a local safety net (gitignored)
- Decision log entry + updated `.gitignore` + content storage rule feedback

**Why:** RA's earlier session crash lost all unstaged *thinking* -- the files survived on disk but the context, reasoning, and partial-state coherence was all in the crashed Claude session. Session 99 diffed the files cold, reverse-engineered the intent from the diffs, and staged a coherent commit. The recovery took most of a night-shift session.

**Still parked (do not mark done):**
- **`brand-callout` and `brand-collection`** still need the WF2 fidelity rules port (R2/R3/R4) like `brand-bold` got. Only path updates were made in the recovery -- the fidelity rewrite itself needs QA testing time RA doesn't have right now. See `feedback_brand_skill_rewrite.md` for the full rewrite brief.
- `brand-content` orchestrator wasn't touched -- it's just routing, probably doesn't need fidelity rules itself.

**How to apply:** When RA mentions "the crashed session," "Karpathy/Nate Herk work," or "the brand skill rewrite," the recovery happened in session 99 and the commit is `fc3bddf` on main. brand-callout and brand-collection fidelity port is still on the backlog -- resume it when RA has QA testing bandwidth. Don't attempt it proactively.
