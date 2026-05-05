---
name: Claude CLI Location
description: Claude Code CLI installed via npm, path for subprocess calls from Python tools
type: reference
related: [feedback_chatbot_architecture.md, project_knowledgebase_informdata.md]
---

Claude Code CLI installed globally via npm:
- Command: `claude` (via npm global)
- Windows path: `C:\Users\RAS\AppData\Roaming\npm\claude.cmd`
- Version: 2.1.91 (as of Apr 2026)
- Uses Max plan auth (no API key needed)

For Python subprocess calls, use the full path since Git Bash PATH doesn't include npm globals:
```python
CLAUDE_PATH = r"C:\Users\RAS\AppData\Roaming\npm\claude.cmd"
```

For long system prompts, use `--system-prompt-file` not `--system-prompt` (CLI arg length limit).
Pipe user message via stdin, not as a positional argument.
