---
name: Chatbot Architecture Lessons
description: Key patterns for building chatbots with claude --print on Max plan -- stdin piping, keyword indexing, greeting bypass
type: feedback
related: [feedback_chatbot_language.md, project_knowledgebase_informdata.md, project_llm_wiki_system.md, reference_claude_cli.md]
---

When building a chatbot using `claude --print` (Max plan), follow these patterns:

**1. Pipe full prompt via stdin, not command-line args**
Windows has a 32K command-line limit. Use `subprocess.run(cmd, input=full_prompt)` instead of passing the prompt as a positional argument. This also makes the context strong enough that CLAUDE.md persona doesn't override it.

**Why:** The KB chatbot failed with FileNotFoundError (misleading) because 78K chars exceeded Windows cmd limit. Stdin has no limit.

**2. Don't fight CLAUDE.md -- flood the context**
Don't try to suppress CLAUDE.md with competing personas. Instead, pipe instructions + knowledge as the full prompt via stdin. When the context is clearly about a specific domain, the model follows the inline instructions over CLAUDE.md.

**Why:** `--system-prompt-file` doesn't suppress CLAUDE.md. `--bare` breaks Max plan auth. The only reliable approach is making the piped content so clearly domain-specific that CLAUDE.md becomes irrelevant.

**3. Keyword-index the knowledge, don't dump everything**
Map files to keyword triggers. Load only relevant files per query (+ base context). Fallback to all files if no keywords match.

**Why:** Sending 78K every query is wasteful even on Max plan (slower responses). Indexed queries use 20-25K instead -- 70% reduction.

**4. Handle greetings without calling Claude**
Detect greetings ("hi", "hello", etc.) and return a canned response instantly. Zero tokens, instant reply.

**How to apply:** Use these patterns for any future chatbot built on `claude --print` with Max plan.
