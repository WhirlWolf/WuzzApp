# Configuration

Server settings live in `wuzapi/.env` (copied from `.env.sample` in [Getting Started](getting-started.md#step-3--configure-your-env-file-in-termux)).

| Variable | What it does |
|---|---|
| `WUZAPI_ADMIN_TOKEN` | Admin token, printed on first run. Treat like a password. |
| `WUZAPI_GLOBAL_ENCRYPTION_KEY` | 32-character key that encrypts stored session data. |
| `WUZAPI_GLOBAL_HMAC_KEY` | 32-character key that signs requests. |
| `SESSION_DEVICE_NAME` | Name shown in WhatsApp's **Linked Devices** (cosmetic). |
| `WUZAPI_PORT` | Server port, default `8080`. If you change it, update every Tasker task with a hardcoded URL (e.g. `WUZ - Connect`, `WUZ - Setup`). |
| `TZ` | Timezone like `Asia/Kolkata`. Affects `%time` in alerts. |
| `WEBHOOK_FORMAT` | Keep as `json`. |
| `WEBHOOK_RETRY_ENABLED` | Retry failed webhook deliveries. |
| `WEBHOOK_RETRY_COUNT` | Number of retries. |
| `WEBHOOK_RETRY_DELAY_SECONDS` | Seconds between retries. |

## The wuzapi user

Users are stored in `dbdata/main.db` (older versions used `users.db`; existing data is migrated automatically). Create one:

```
sqlite3 dbdata/main.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
```

Or log into the dashboard with the admin token and tap **Add Instance**.

- `id` / `name`: labels shown in the dashboard.
- `token`: what you log into `http://localhost:8080/dashboard/` with. Not the admin token.

> **Second account:** the Tasker project uses one token at a time. To switch, update the token the project uses and reconnect. Not officially supported (see [FAQ](faq.md)).

The dashboard at `http://localhost:8080/dashboard/` is mainly for first-time linking ([Getting Started](getting-started.md#step-2--link-your-whatsapp)). After that, Tasker handles the connection.

## Regenerating keys

If a key or token leaks, replace it right away:

1. Stop the server.
2. Put a new random 32-character string in `.env`.
3. Restart with `./wuzapi`.
4. If you changed the admin token or the encryption/HMAC keys, relink your device. Old session data was encrypted with the old key.

See also [Safety & Privacy](safety-and-privacy.md#securing-your-setup).
