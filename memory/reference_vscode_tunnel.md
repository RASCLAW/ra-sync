---
name: VSCode Tunnel Setup
description: VSCode remote tunnel (dubery-dev) runs as a Windows service, auto-starts on boot. Access via vscode.dev.
type: reference
related: [feedback_loadout_remote_status.md, reference_backup_system.md, reference_vscode_extension_orphan_bug.md, reference_cloudflare_migration.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
VSCode tunnel `dubery-dev` is installed as a Windows service via `code-tunnel tunnel service install`.

- Auto-starts on boot, auto-restarts on crash
- Access: vscode.dev -> Remote Explorer -> dubery-dev
- Claude Code extension (anthropic.claude-code) is installed and works remotely
- Power settings: AC lid close = do nothing, DC = do nothing + 30min sleep
- Management commands: `code-tunnel tunnel status`, `code-tunnel tunnel service log`, `code-tunnel tunnel service uninstall`
- Skill: `/vscode-tunnel` (for manual start if service is removed)
