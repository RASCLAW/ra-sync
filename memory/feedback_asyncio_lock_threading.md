---
name: asyncio.Lock Breaks Per-Request Event Loops
description: asyncio.Lock at module init binds to first event loop; fails on all subsequent Flask per-request threads — use threading.Lock instead
type: feedback
related: [reference_cc_command_center.md, feedback_cc_dual_instance.md]
originSessionId: 125db2c5-5c3c-4897-893c-3bc8a1f33702
---
In `AgentSession.__init__()`, `self._lock = asyncio.Lock()` is created with no running event loop. The first `.ask()` call binds that lock to its event loop. Every subsequent Flask request creates a new thread with a new event loop — and hits `RuntimeError: Lock is bound to a different event loop`.

Fix: `self._lock = threading.Lock()` + `with self._lock:` (sync context manager, works inside async functions).

**Why:** Flask spawns a new thread per request. Each thread runs `asyncio.new_event_loop()`. `asyncio.Lock()` is per-loop — it cannot be shared across loops.

**How to apply:** Any singleton with an `asyncio.Lock` that gets called from Flask routes (or any threading context) must use `threading.Lock()` instead. The `async with` → `with` change is required too.
