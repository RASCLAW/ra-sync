# Behavioral Rules Index

Feedback memories on how to operate -- working style, session rhythm, tool quirks, communication, safety. Loaded on-demand when the main MEMORY.md points here.

- [User PATH Wipe Recovery](feedback_user_path_wipe_2026_05_20.md) -- HKCU\Environment\Path can vanish; setx Python312+npm dirs; VBS shim for detached cmd with TTY
- [Bash Login PATH Inheritance](feedback_bash_login_path_inheritance.md) -- `bash -l -c "claude ..."` from a .bat doesn't reliably inherit Windows user PATH; use full binary paths from cmd, skip the bash hop
- [Savepoint Sweet Spot](feedback_savepoint_sweetspot.md) -- call /savepoint at 75% context (~150k) on Sonnet 200k
- [Settings Self-Modification Auth](feedback_settings_self_modification.md) -- settings.json edits need explicit RA auth each session
- [autoCompactWindow Setting](reference_autocompact_window.md) -- token count field (100k-1M), currently 185k
- [Log Double-Check](feedback_log_doublecheck.md) -- confirm with RA before writing /log outputs
- [Verify Before Ask](feedback_verify_before_ask.md) -- grep/check local files before asking RA to fetch anything
- [Diagnostic Depth](feedback_diagnostic_depth.md) -- batch parallel, synthesize, pivot on failure
- [Read Code, Don't Screenshot](feedback_read_code_not_screenshot.md) -- read source to orient, screenshots only to prove visual result to RA
- [Simple Flow > Scroll-Scrub](feedback_simple_flow_beats_scroll_scrub.md) -- normal doc flow + fixed bg beats data-enter/leave visibility dispatchers for scroll sites
- [CF Edge Cache Bust](feedback_cloudflare_edge_cache_bust.md) -- CF edges cache per geography; cache-bust per-file with ?v=<tag> when iterating
- [Kraft Not in Ambient BG](feedback_kraft_not_in_ambient_bg.md) -- kraft product shots only in product cards; ambient bgs use UGC + editorial
- [Mid-Session Saves](feedback_mid_session_save.md) -- always write memory on save points, don't ask
- [Resume Pointer](reference_resume_pointer.md) -- RESUME.md = "where was I" single source of truth, overwrites every savepoint
- [Date/Time Tracking](feedback_datetime_tracking.md) -- always note current date/time, real timestamps
- [Chatbot Language](feedback_chatbot_language.md) -- 95% English, minimal Filipino sprinkles
- [Chatbot No Model Codes](feedback_chatbot_no_model_codes.md) -- never mention D518/D918/D008 in replies, product name + color only
- [Chatbot No Peso Prefix](feedback_chatbot_no_peso_prefix.md) -- plain numbers (599, 1200, 100), no P/PHP/₱
- [Chatbot Address By Name](feedback_chatbot_address_by_name.md) -- use customer first name when known, sparingly throughout conversation
- [Metro Manila Only](feedback_metro_manila_only.md) -- ads target MM only; provincial = organic follower, handoff over close
- [Direct Links Preference](feedback_direct_links.md) -- send raw URLs, not navigation steps
- [Interrupt Handling](feedback_interrupts.md) -- when RA interrupts, ask if can continue
- [Image Review Before Use](feedback_image_review.md) -- always let RA pick images, build gallery
- [Visual Product Inspection](feedback_visual_product_inspection.md) -- Read hero shots before writing copy
- [Content Storage Rule](feedback_content_storage_rule.md) -- git=code, Drive=content, local=disposable
- [Multi-Session Safety](feedback_multi_session_safety.md) -- don't commit files another session is editing
- [Multi-Window Workflow](feedback_multi_session_workflow.md) -- deferred push via /closeout --defer + /sendit
- [Session Rename Drift](feedback_session_rename_drift.md) -- suggest /rename when topic drifts
- [Claude Code Permissions](feedback_claude_code_permissions.md) -- no quotes in Bash patterns, C:/ paths
- [Sonnet Delegation](feedback_sonnet_delegation.md) -- when to route to Sonnet vs keep in Opus
- [Document Testing](feedback_document_testing.md) -- always document steps, inputs, results, and insights during testing
- [Closeout Format](feedback_closeout_format.md) -- one-liner default, full ADR for architectural only
- [Loadout Remote + Sessions](feedback_loadout_remote_status.md) -- check tunnel/power + orphan detection
- [Pillow Pixel Art](feedback_pillow_pixel_art.md) -- pixel art not smooth vectors
- [Make Router Paths](feedback_make_router_paths.md) -- fallback routes need explicit exclusion filters
- [Make Module Versions](feedback_make_module_versions.md) -- always use latest, old ones break
- [AI Prompt Verbatim Block](feedback_ai_prompt_verbatim.md) -- Make AI parrots unless told not to
- [Batch Ingest Pattern](feedback_batch_ingest_pattern.md) -- 3+ sources → parallel Sonnet subagents for raw+summary, main Opus consolidates INDEX/log/memory/backlog
- [Pro Plan Workflow](feedback_pro_plan_workflow.md) -- budget-mode rules: no Opus, no /loop, single session, API key danger, verified billing facts
- [Image Review Dashboard](reference_review_dashboard.md) -- Stage 1 + Stage 2 URLs (review./tag.duberymnl.com), manifest schema, 5-tag system
- [Content Distribution System](project_content_distribution_system.md) -- phased plan to wire manifest into story/feed/ad channels

## Claude Code Architecture
- [4-Layer Architecture](feedback_claude_code_layers.md) -- L1-L4, context optimization applied session 114
- [Brad Usage Limits](../../../../projects/EA-brain/references/summaries/brad-claude-code-usage-limits.md) -- progressive disclosure, MCP hygiene, autoCompact 75%
