---
name: Command Center Phase 2 Scoping
description: Session 131 scoping state for Content Gen tab. Pill form locked, two-randomizer wrinkle identified, three-part plan sketched but not written. Next session picks up here.
type: project
related: project_command_center_phase1_complete.md, reference_command_center.md, reference_pipeline_skill_chain.md, feedback_form_always_randomizes.md
originSessionId: a1405cce-bffb-44b2-985e-90425a6b5c11
---
## Status
SUPERSEDED by session 133 build. Content Gen tab shipped with a simplified design (Mode/Type/Count + Direction chat instead of per-dimension pill rows). Bespoke mode added. This file retained for historical context on the scoping decisions that led there.

**Why:** Session 131 spec was overengineered. RA simplified to 3 pills + chat direction during session 133 execution. Bespoke mode emerged organically when RA pasted concept images.

**How to apply:** Content Gen tab is built. Remaining Phase 2: Marketing tab + proactive bot bubbles. See `reference_command_center.md` for current state.

## Locked decisions (form shape)
- **Flat form, all fields visible** — not a stepper, not a chat-first chip builder.
- **Pill chips, not dropdowns.** One row per field, pills are the values.
- **Multi-select per row.** Lock 1-N pills → randomizer picks among the locked set. Lock zero → randomizer picks from the full bank.
- **Count uses +/- stepper**, not a pill row (pills cap the values, stepper is flexible).
- **Blank field = randomize.** Every generation path routes through the randomizer CLI. The form never builds a prompt directly. See `feedback_form_always_randomizes.md`.

## Locked field set
Mode / Product / Category / Count / Location / Scene.

**Mode pill reshapes the field set:**
- **Mode=UGC** → Product / Category / Location / Scene pills visible
- **Mode=Brand** → Product / Skill (callout/bold/collection) / Layout pills visible (no Location/Scene — brand content has layouts instead)

Same tab, two configurations driven by the Mode pill.

## The two-randomizer wrinkle (unresolved)
UGC mode and Brand mode use different randomizers with different CLI surfaces. The form has to route per Mode:

- **`tools/image_gen/v3_randomizer.py`** (UGC) — CLI: `--product`, `--category`, `--count`, `--seed`. **Missing:** `--location`, `--pose`. Location/scene banks exist internally (LOCATIONS_PERSON, LOCATIONS_PRODUCT, POSES_WEARING, etc.) but are not exposed as flags.
- **`tools/image_gen/batch_randomizer.py`** (Brand + cross-skill mix) — CLI: `--type {ugc|brand-callout|brand-bold|brand-collection|mix}`, `--count`, `--seed`, `--no-history`. Products cycle automatically (`PRODUCTS[i % len(PRODUCTS)]`); layouts picked per-skill (CALLOUT_LAYOUTS, BOLD_LAYOUTS, COLLECTION_LAYOUTS).

**Verify against code before planning:** banks shift (Rasta Red prodref issue, headband bans, etc.). Re-run `grep '^[A-Z_]\+ = \[' tools/image_gen/v3_randomizer.py` to confirm current lists before wiring pill vocab.

## Three-part plan sketch (not written to .tmp/plan.md yet)
1. **Extend `v3_randomizer.py`** with `--location` and `--pose` CLI flags. Accept one or more comma-separated values; intersect with the category-appropriate bank; fall back to full bank on empty. Aligns with `/ugc-pipeline` Step 3 rule "never override in the prompt, edit the randomizer."
2. **Build the pill form tab** (`templates/tabs/content_gen.html` + `static/js/content_gen.js`). Multi-select pill rows, stepper for count, Mode pill reshapes the visible rows. Submit button packages locks into an args object, POSTs to an endpoint that constructs the CLI invocation and pipes it to the existing `AgentSession` via `/api/agent/chat`. Stream output into a panel inside the tab (not the floating FAB).
3. **Mode-aware pill rows.** UGC config shows Location + Scene pills; Brand config shows Skill + Layout pills instead. Handled in the frontend — backend just receives the args object and knows which randomizer to call based on Mode.

## Parked for Phase 2 polish, not MVP
- **Marketing tab "agent thinking" window** — streams SDK intermediate messages (tool calls, reasoning blocks), not just final text. Portfolio prop. Build after Content Gen MVP is shipped.
- **Proactive bot bubbles** (event-driven + periodic safety net) — Phase 2 item #3 in the phase1_complete memory. Not touched this session.

## Open questions for next session
- Does extending `v3_randomizer.py` break existing `/ugc-pipeline` calls? (Should be backward compatible if flags default to None, but verify.)
- Should Brand mode Product pills be multi-select too, or single-select? (batch_randomizer cycles all products by default; a lock would need new flag logic there as well.)
- Where does the streaming output panel sit in the Content Gen tab layout? Above the pill form, below, or a split view?

## Flagged discrepancy (unresolved, carried from session 130)
Meta Ads monitor reports 5 ACTIVE adsets vs `current-priorities.md` item 1h saying ads are paused. Still waiting on RA eyeball.
