---
name: Register Rebind Import Gotcha
description: Python module-level list reassignment does NOT propagate to callers who already imported the list. Mutate in place instead.
type: feedback
related: reference_command_center.md
originSessionId: 65f454a5-f69c-4ea7-9925-80aa4bafc8cc
---
Module-level lists/dicts imported via `from pkg import THING` are **aliases**, not live references. If the module's own code rebinds `THING = [...]` (new object), importers still see the old list.

**Why:** this matters for any registry pattern where the module provides both "the list" and a "register()" function that mutates it. The classic bug:

```python
# monitors/__init__.py
SERVICES = []

def register(name, fn):
    global SERVICES
    SERVICES = [x for x in SERVICES if x[0] != name]  # <-- rebinds, breaks aliasing
    SERVICES.append((name, fn))

# monitors/registry.py
from monitors import register
register("foo", foo_check)   # writes to the NEW list

# app.py
from monitors import SERVICES   # imported BEFORE register() ran
# SERVICES here is the ORIGINAL empty list, never updated
```

**Symptom:** endpoints that iterate SERVICES raise `KeyError` or silently return empty results. Caught session 130 during command-center build — `/api/monitor/status` returned `KeyError: 'chatbot_tg'` because only the first couple of registrations survived a comprehension rebind.

**Fix:** mutate in place. Never rebind module-level containers that anyone imports by name.

```python
def register(name, fn):
    for i, (n, _) in enumerate(SERVICES):
        if n == name:
            SERVICES[i] = (name, fn)
            return
    SERVICES.append((name, fn))
```

**How to apply:** any time you build a registry in a Python module and expose it both as data (`from mod import registry`) and as an API (`mod.register(...)`) — the API must mutate, never reassign. Same rule for dicts (`d[k] = v` ok, `d = {...}` not ok) and sets. Rule of thumb: if importers rely on the name, don't rebind it.
