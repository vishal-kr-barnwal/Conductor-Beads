---
description: "DEPRECATED: Use bd dolt push instead. Synchronize issues with Dolt remote."
argument-hint: "[DEPRECATED]"
---

> **⚠️ DEPRECATED (v0.56+):** `bd sync` is now a no-op. Use `bd dolt push` and `bd dolt commit` instead.

The JSONL-based sync pipeline has been completely removed. Dolt-native push/pull via git remotes is the only sync mechanism.

## Replacement Commands

| Old Command | New Command |
|-------------|-------------|
| `bd sync` | `bd dolt push` |
| `bd sync --flush-only` | `bd dolt commit -m "message"` |
| `bd sync --no-push` | (automatic with Dolt server) |

## Current Dolt Workflow

```bash
# Commit pending changes
bd dolt commit -m "Session work"

# Push to remote
bd dolt push
```

Most users should rely on the Dolt server's automatic commits (one per write command). Use `bd dolt push` at session end to ensure data reaches the remote.
