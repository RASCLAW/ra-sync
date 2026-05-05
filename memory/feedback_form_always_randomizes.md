---
name: Command Center Forms Always Route Through Randomizer
description: Command Center generation forms never build prompts directly. Every field is optional, blank = randomize, locks flow through randomizer CLI flags not prompt hints.
type: feedback
related: reference_pipeline_skill_chain.md, project_command_center_phase2_scoping.md, reference_command_center.md, feedback_use_skills_for_content.md
originSessionId: a1405cce-bffb-44b2-985e-90425a6b5c11
---
**Rule:** Command Center generation tabs (Content Gen, Marketing, future batch tools) are structured controls over the randomizer, not prompt builders. Every pill/field is optional. Blank means "randomize this dimension." Locked values flow to the randomizer as CLI flags — never as natural-language hints in the agent prompt.

**Why:** RA locked this during Phase 2 scoping session 131. The reasons stack:
1. `/ugc-pipeline` Step 3 already mandates it: "Do NOT pick scene dimensions manually. If a value looks wrong in the randomizer output, edit `tools/image_gen/v3_randomizer.py` — never override in the prompt." The form must respect the same rule.
2. Randomizer-as-single-source-of-truth means dedup logic (headline history, layout history, category rotation) can't be bypassed by accident.
3. Locks-via-prompt are unreliable — Claude may soften or ignore a natural-language "use location CAFE_TABLE" hint. CLI flags are deterministic.

**How to apply:**
- **Form design:** every field defaults to "no lock" (randomize). No field is required except count (and count gets a sensible default). No "manual prompt" escape hatch.
- **Backend routing:** the endpoint builds a CLI invocation (`python tools/image_gen/v3_randomizer.py --product bandits-blue --category UGC_SELFIE --count 3`) or equivalent, then hands the resulting assignments to the agent session for prompt building + validation + generation. It does NOT compose a prompt saying "generate a Bandits Blue selfie in a cafe."
- **When the randomizer doesn't support a lock:** don't add it to the form as a prompt hint. Add the CLI flag first, then expose the pill. This is why Phase 2 blocks on extending `v3_randomizer.py` with `--location` + `--pose` before the Location/Scene pills get wired.
- **If a Brand-side form is added later:** same rule — route through `batch_randomizer.py` with locks as flags. Invent new flags on that script if needed; don't freelance in the prompt.

**Applies to:** Content Gen tab, Marketing tab creative picker, any future batch-generation UI anywhere in the Command Center. Does not apply to the floating chat FAB — that's a free-form agent chat, different tool.
