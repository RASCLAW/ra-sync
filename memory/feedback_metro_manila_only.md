---
name: DuberyMNL Metro Manila Only
description: Target audience is Metro Manila only. Provincial buyers are always organic followers, never ad-targeted. When detected, prioritize handoff over trying to close.
type: feedback
related:
  - project_chatbot_employee_discipline.md
  - project_messenger_strategy.md
  - reference_dubery_crm_sheet.md
originSessionId: f2b83343-be79-40bb-b8a6-851c6856aee3
---
DuberyMNL ads target **Metro Manila only**. Any provincial inquiry (Tawi-Tawi, Batangas, Ilocos, etc.) is therefore an **organic follower** who saw a post, not an ad click.

**Why:** Confirmed 2026-04-16 by RA after the Alkabir Tawi-Tawi incident where the chatbot spiraled trying to enforce provincial prepayment policy against a customer who kept insisting on COD. Alkabir was organic, not ad-driven. Context: current-priorities #1 (DuberyMNL chatbot production) + Messenger-first strategy — ads stay MM to keep COD as the default conversion path.

**How to apply:**
- **Chatbot behavior:** provincial customer = low-priority segment. Deliver the prepayment policy **once**, then hand off if they push back. Don't design chatbot flows that assume provincial buyers will become large volume. (This is exactly what `prepay_provincial` policy one-shot enforces — see `project_chatbot_employee_discipline.md`.)
- **Ad design:** never mention nationwide shipping as a headline — keep MM/NCR framing so viewers self-qualify. Provincial conversion requires manual nurture, not bot.
- **Analytics framing:** when comparing bot-handled vs. RA-handled convos, flag provincial ones separately — they should handoff by design, not close in-bot.
- **Portfolio framing:** this is part of the discipline story — *the bot knows its own ICP boundaries and doesn't fight them.*
