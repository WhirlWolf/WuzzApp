# Configuration Reference

All server-side configuration lives in `wuzapi/.env`. This is created by copying `.env.sample` during [Getting Started](getting-started.md#step-3--configure-your-env-file-in-termux).

| Variable                       | Description                                                                                                                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `WUZAPI_ADMIN_TOKEN`           | Admin-level token for wuzapi, printed on first run. Needed for user management via wuzapi's API. Treat like a password.                                |
| `WUZAPI_GLOBAL_ENCRYPTION_KEY` | 32-character key used to encrypt stored session data. Printed on first run.                                                                            |
| `WUZAPI_GLOBAL_HMAC_KEY`       | 32-character key used to sign requests. Printed on first run.                                                                                          |
| `SESSION_DEVICE_NAME`          | The device name shown in WhatsApp's **Linked Devices** list (e.g. `macOS`). Purely cosmetic.                                                           |
| `WUZAPI_PORT`                  | Port the wuzapi server listens on. Default `8080`. Change if that port is already in use. If you change it, update every Tasker task in the WuzzApp project that hardcodes the dashboard/API URL (e.g. `WUZ - Connect`, `WUZ - Setup`, and any task using `http://localhost:8080/...`) to use the new port instead. |
| `TZ`                           | Your timezone, in `Area/City` format (e.g. `Asia/Kolkata`). Affects timestamps used in alert templates like `%time`.                                   |
| `WEBHOOK_FORMAT`               | Format used when wuzapi posts events to WuzzApp's webhook. Keep as `json`.                                                                             |
| `WEBHOOK_RETRY_ENABLED`        | Whether wuzapi retries delivering an event if the webhook call fails.                                                                                  |
| `WEBHOOK_RETRY_COUNT`          | Number of retry attempts.                                                                                                                              |
| `WEBHOOK_RETRY_DELAY_SECONDS`  | Delay between retries, in seconds.                                                                                                                     |

## The wuzapi user

Separately from the `.env` file, wuzapi keeps a small SQLite database of users at `dbdata/users.db`. The initial user is created during setup:

```
sqlite3 dbdata/users.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
```

- `id` / `name` — arbitrary labels, shown in wuzapi's dashboard.
- `token` — the per-user token you log into `http://localhost:8080/dashboard/` with. This is different from `WUZAPI_ADMIN_TOKEN`.

You can create additional users the same way if you want to run more than one WhatsApp session.

> **Using a second account in practice:** the Tasker project's tasks (e.g. `WUZ - Connect`) are wired to a single active token/session at a time. To actually switch which WhatsApp account is active, you'd update the token the project uses to log into wuzapi and reconnect — there's no built-in UI for running two sessions side-by-side in Tasker. This isn't officially supported (see the [FAQ](faq.md)); treat it as an advanced, manual setup rather than a supported feature.

These tokens and keys grant access to your linked WhatsApp session — see [Safety & Privacy → Securing your setup](safety-and-privacy.md#securing-your-setup) for handling and what to do if any of them leak.

## Where the dashboard lives

With the server running, `http://localhost:8080/dashboard/` gives you a browser UI to connect/disconnect the session and view its QR/pairing state. This is mainly needed during initial linking (see [Getting Started](getting-started.md#step-2--link-your-whatsapp)) — day-to-day, WuzzApp's Tasker tasks handle connection state for you.

## Regenerating keys

If any key or token is ever exposed, regenerate it immediately:

1. Stop the server.
2. Edit `.env` and replace the relevant value with a new random 32-character string.
3. Restart the server with `./wuzapi`.
4. If you changed `WUZAPI_ADMIN_TOKEN` or the encryption/HMAC keys, you'll likely need to relink your device — existing session data was encrypted with the old key.
