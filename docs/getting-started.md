# Getting Started

Takes about 20-30 minutes and ~500MB of data.

| Part | What it does | Runs in |
|---|---|---|
| **wuzapi** | WhatsApp connection server | Termux |
| **Tasker project** | Reacts to events, sends alerts, runs your tasks | Tasker |

## Prerequisites

- [Termux](https://f-droid.org/repo/com.termux_1022.apk) from F-Droid, **not** the Play Store
- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm) (paid)

## Step 1 — Install and build wuzapi (in Termux)

1. Open Termux and allow notifications (keeps the server alive).
2. Update packages:
   ```
   apt update && apt upgrade -y
   ```
3. Install dependencies:
   ```
   apt install git golang sqlite -y
   ```
4. Let Tasker send commands to Termux:
   ```
   sed -i '/^#\s*allow-external-apps\s*=\s*true/s/^#\s*//' ~/.termux/termux.properties
   termux-reload-settings
   ```
5. Grant storage access:
   ```
   termux-setup-storage
   ```
6. Build and run wuzapi (the build takes a few minutes):
   ```
   git clone https://github.com/asternic/wuzapi
   cd wuzapi
   go build
   cp .env.sample .env
   nano .env
   ```
   .env file will be shown, use following button combinations to edit the file:
   `Alt+A`, `Alt+/`, `Ctrl+K`.

7. Copy and fill following code with your keys (tip: use password generator) and your timezone (e.g. `Asia/Kolkata`).
```
# .env
# Server Configuration
WUZAPI_PORT=8080
WUZAPI_ADDRESS=0.0.0.0

# Optional: discard all inbound group events before media download, history
# persistence and webhook delivery. Existing behavior is unchanged by default.
WUZAPI_IGNORE_GROUP_EVENTS=false

# Token for WuzAPI Admin
WUZAPI_ADMIN_TOKEN=your_admin_token_here

# Encryption key for sensitive data (32 bytes for AES-256)
WUZAPI_GLOBAL_ENCRYPTION_KEY=your_32_byte_encryption_key_here

# Global HMAC Key for webhook signing (minimum 32 characters)
WUZAPI_GLOBAL_HMAC_KEY=your_global_hmac_key_here_minimum_32_chars

# "json" or "form" for the default
WEBHOOK_FORMAT=json

# Route webhook deliveries through the per-user proxy when configured (default: true)
WUZAPI_WEBHOOK_USE_PROXY=true

# Attachment work concurrency (positive integer; default 2)
WUZAPI_MEDIA_CONCURRENCY=2
# Defaults to the OS temp directory. Use a disk-backed writable mount, not tmpfs.
# WUZAPI_MEDIA_TMPDIR=/var/lib/wuzapi/media

# WuzAPI Session Configuration
SESSION_DEVICE_NAME=macOS
# "available" (legacy default) or "unavailable" (preserves phone push notifications)
WUZAPI_AUTO_PRESENCE=unavailable

# Trim on every Nth saved message per user and chat (default 1, every message)
WUZAPI_HISTORY_TRIM_INTERVAL=1
# When every proxy pool entry is full: "block" (default, refuse to connect) or "direct" (connect without a proxy)
WUZAPI_PROXY_POOL_FALLBACK=block

TZ=Asia/Kolkata

# Minimum WuzAPI log level. Empty preserves legacy verbosity; separate from -wadebug.
# Accepted: trace, debug, info, warn, error, fatal, panic.
LOG_LEVEL=

# Recover closed sockets after send failures (no automatic message retry).
WUZAPI_SOCKET_RECOVERY=true

# Repair app state collections that fail to sync (full resync, then a request to the phone).
WUZAPI_APPSTATE_RECOVERY=true

WEBHOOK_RETRY_ENABLED=true
WEBHOOK_RETRY_COUNT=1
WEBHOOK_RETRY_DELAY_SECONDS=30
```
   What each setting does: [Configuration](configuration.md).
   
8. Paste the code, save with `Ctrl+O`, `Enter`, then exit with `Ctrl+X`.

9. Create WhatsApp user. `id`, `name` and `token` can be anything, just remember the token:
   ```
   sqlite3 dbdata/main.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
   ```
   >
   > Alternative: start the server (step 10) , open `http://localhost:8080/dashboard/` in browser, log in as admin with the **admin token** you entered in .env file and tap **Add Instance**.

10. Start the server:
   ```
   ./wuzapi
   ```

## Step 2 — Link your WhatsApp

1. With wuzapi running, open `http://localhost:8080/dashboard/` in your browser.
2. Log in with your **user token** (not the admin token).
3. Tap **Connect**.
4. Import the Tasker project: [WuzzApp on TaskerNet](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)
5. In Tasker, run **WUZ - Project Setup**. It copies a linking code.
6. In WhatsApp, tap on menu (elipsis button) > Linked devices > Link a device > Link with phone number instead > Enter code copied to clipboard (You may also get a prompt in WhatsApp; follow that instead.)

## Step 4 — Finish setup in Tasker

1. Go to **Tasker → App Info → Permissions → Additional Permissions** and enable **Run commands in Termux environment**.
2. Run **WUZ - Project Setup** to register the webhook.

🎉 Done. Next: [User Guide](user-guide.md) or [Event Preferences](event-preferences.md).

Android may kill Termux in the background: see [Background reliability](troubleshooting.md#background-reliability).
