---
name: Claude Code Permission Quirks
description: Three gotchas when writing settings.json permission rules -- quote normalization, Windows path format, and no hot-reload.
type: feedback
related:
  - reference_rasclaw_mobile_permissions.md
  - feedback_powershell_from_bash.md
originSessionId: eba69f11-3dc7-4a1f-a896-98c4118a1506
---
Writing Bash/Read patterns in `~/.claude/settings.json` is trickier than it looks. Three things that wasted iterations in session 99 before figuring them out.

## 1. Bash patterns with `"` don't match

**Why:** Claude Code normalizes the command before matching against allow patterns -- shell quotes get stripped. A pattern like `Bash(cp "*inbox*" *)` expects literal quotes that aren't there at match time, so it never fires even though the actual command has `cp "C:/Users/.../inbox/file.jpg" ...`.

**How to apply:** Don't include `"` in Bash permission patterns. Use unquoted globs like `Bash(cp *inbox* *)` or just `Bash(cp *)`. If the real command needs shell quoting, ignore those quotes when writing the pattern.

## 2. Claude's internal Windows path format is `C:/Users/...` (forward-slash Windows)

**Why:** Not git-bash `//c/Users/...`. The existing `Read(//tmp/**)` rule only works because `//tmp` is a Unix-native path. For Windows, Claude's matcher sees paths like `C:/Users/RAS/.claude/channels/telegram/inbox/file.jpg` -- forward slashes with the drive letter.

**How to apply:** Prefer path-agnostic globs like `Read(**/channels/telegram/inbox/**)` anchored on a unique folder name. They match regardless of how the path got normalized. Avoid `//c/...` and backslash `C:\\...` forms for new rules.

## 3. settings.json is NOT hot-reloaded

**Why:** Permission rules are loaded once per Claude Code session start. Editing settings.json mid-session has zero effect on the running process. This matters most for the Rasclaw telegram session, which is a SEPARATE Claude Code instance from the VSCode one -- edits from VSCode never reach it until the telegram session is killed + relaunched.

**How to apply:** After editing permissions for a long-running session (channels, background subagents), you must restart that process. `~/.claude/scripts/start-rasclaw.bat` relaunches the telegram session. If RA says "still prompting after the edit," the answer is usually "restart the process that's hitting the prompt."
