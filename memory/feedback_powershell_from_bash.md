---
name: PowerShell from Git Bash Quirks
description: Git Bash mangles PowerShell `$_` + schtasks /Create hits Access Denied. Use .ps1 files for anything non-trivial.
type: feedback
related:
  - feedback_claude_code_permissions.md
  - reference_chatbot_autostart.md
originSessionId: dcf22698-e101-4a70-8d5e-c94d2da0ed02
---
Two failures waste iterations when calling PowerShell or schtasks from Git Bash on Windows. Always prefer `.ps1` script files over inline `-Command` for anything non-trivial.

## 1. Git Bash interprets `$_` as bash's last-arg variable

**Why:** Inside `powershell -Command "... $_.Foo ..."`, Git Bash expands `$_` (its own last-argument variable) before passing the string to PowerShell. PowerShell then sees a mangled token like `extglob.Foo` instead of `$_.Foo` and fails with `Unexpected token`.

**How to apply:** Whenever a PowerShell command uses `$_` (ForEach-Object, Where-Object, Select-Object computed properties), put the logic in a `.ps1` file and call it with `powershell -NoProfile -ExecutionPolicy Bypass -File path.ps1`. The file contents are read verbatim by PowerShell, bypassing bash expansion.

```bash
# BAD (bash mangles $_)
powershell -NoProfile -Command "Get-Process | Where-Object { $_.Name -eq 'python' }"

# GOOD (bash hands the file off untouched)
powershell -NoProfile -ExecutionPolicy Bypass -File check.ps1
```

## 2. `schtasks /Create` errors "Access denied" from Git Bash even for user-scoped tasks

**Why:** The `schtasks.exe /Create` verb appears to require elevation for some task types even when creating a task that runs as the current user. From Git Bash (non-elevated), it fails with `ERROR: Access is denied.`

**How to apply:** Use PowerShell's `Register-ScheduledTask` cmdlet instead. With `-RunLevel Limited -LogonType Interactive`, it creates user-scoped at-logon tasks with no admin prompt. Working pattern lives in [cloud-run/install-autostart.ps1](cloud-run/install-autostart.ps1) — copy it as a template when adding more at-logon tasks.

## 3. Default `open()` in Python uses cp1252 on Windows, chokes on UTF-8 files

**Why:** When bash-invoked Python reads a file with UTF-8 characters (em-dashes, arrows, smart quotes, emoji), `open(path)` fails with `UnicodeDecodeError: 'charmap' codec can't decode byte 0x90`. The default encoding is cp1252, not UTF-8.

**How to apply:** Always pass `encoding='utf-8'` when reading source files in ad-hoc Python snippets:
```python
ast.parse(open('conversation_engine.py', encoding='utf-8').read())
```
