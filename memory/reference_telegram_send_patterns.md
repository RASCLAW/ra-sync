---
name: Telegram Send Patterns (Rasclaw)
description: How to push messages, photos, and rich content to RA's Telegram via Rasclaw bot outside the normal plugin channel flow.
type: reference
related: [project_rasclaw_telegram.md, reference_rasclaw_mobile_permissions.md]
originSessionId: eba69f11-3dc7-4a1f-a896-98c4118a1506
---
Send content to RA on Telegram without going through the `--channels plugin` flow. Useful when you want to push a proactive message, attach a locally-generated image, or deliver research output.

## Setup
- Token file: `~/.claude/channels/telegram/.env` (var: `TELEGRAM_BOT_TOKEN`)
- RA's chat ID: `1762124488`
- Bot username: `Rasclaw01_bot`

## Load env (Git Bash quirk)
`source` alone doesn't export vars to Python subprocess. Use:
```bash
set -a; source ~/.claude/channels/telegram/.env; set +a
```
Then `python - <<'PY' ... PY` can read `os.environ["TELEGRAM_BOT_TOKEN"]`.

## Send text (HTML parse_mode)
```python
import urllib.request, json, os
token = os.environ["TELEGRAM_BOT_TOKEN"]
data = {"chat_id": 1762124488, "text": "<b>Hello</b>", "parse_mode": "HTML",
        "disable_web_page_preview": True}
req = urllib.request.Request(
    f"https://api.telegram.org/bot{token}/sendMessage",
    data=json.dumps(data).encode(),
    headers={"Content-Type": "application/json"})
urllib.request.urlopen(req, timeout=15)
```

## Send photo with caption (multipart)
Telegram `sendPhoto` needs multipart/form-data. Build manually with a custom boundary -- no need for `requests` library.

```python
boundary = "----RAboundary" + os.urandom(8).hex()
body = b''
body += f'--{boundary}\r\nContent-Disposition: form-data; name="chat_id"\r\n\r\n{chat_id}\r\n'.encode()
body += f'--{boundary}\r\nContent-Disposition: form-data; name="caption"\r\n\r\n{caption}\r\n'.encode()
body += f'--{boundary}\r\nContent-Disposition: form-data; name="parse_mode"\r\n\r\nHTML\r\n'.encode()
body += f'--{boundary}\r\nContent-Disposition: form-data; name="photo"; filename="img.png"\r\nContent-Type: image/png\r\n\r\n'.encode()
body += photo_bytes + b'\r\n'
body += f'--{boundary}--\r\n'.encode()

req = urllib.request.Request(
    f"https://api.telegram.org/bot{token}/sendPhoto",
    data=body,
    headers={"Content-Type": f"multipart/form-data; boundary={boundary}"})
```

## HTML escape gotchas
- Escape `&` as `&amp;` in URLs or TG rejects the message
- Only these tags allowed: `<b>`, `<i>`, `<u>`, `<s>`, `<code>`, `<pre>`, `<a>`
- No `<br>` -- use real `\n`

## Caption limit
- Text messages: 4096 chars
- Photo captions: 1024 chars (send longer text as follow-up message)
