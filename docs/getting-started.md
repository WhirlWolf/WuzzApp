# Getting Started

Takes about 20-30 minutes and ~500MB of data (mostly the Go toolchain).

**Three parts:**

| Part | What it does | Runs in |
|---|---|---|
| **wuzapi** | WhatsApp connection server | Termux |
| **Tasker project** | Reacts to events, sends alerts, runs your tasks | Tasker |
| **Event Preferences UI** | Per-event alert settings | Inside the project |

Everything runs on your phone. Nothing leaves it unless you add a webhook.

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
   ./wuzapi
   ```
   > **Important:** On first run it prints an **admin token**, **global encryption key** and **global HMAC key**. Copy them for Step 3 and treat them like passwords ([Safety & Privacy](safety-and-privacy.md#securing-your-setup)).

   Press `Ctrl+C` to stop it.

7. Create your WhatsApp user. `id`, `name` and `token` can be anything, just remember the token:
   ```
   sqlite3 dbdata/main.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
   ```
   > Users live in `dbdata/main.db`. Older wuzapi versions used `users.db`, which is no longer read.
   >
   > Alternative: start the server, open `http://localhost:8080/dashboard/`, log in with the **admin token** and tap **Add Instance**.

8. Start the server:
   ```
   ./wuzapi
   ```
9. Import the Tasker project: [WuzzApp on TaskerNet](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)

## Step 2 — Link your WhatsApp

1. With wuzapi running, open `http://localhost:8080/dashboard/` in your browser.
2. Log in with your **user token** from Step 1.7 (not the admin token).
3. Tap **Connect**.

4. In Tasker, run **WUZ - Project Setup**. It copies a linking code.
5. In WhatsApp, go to **Linked Devices → Link with Phone Number** and paste the code. (You may also get a prompt in WhatsApp; follow that instead.)

## Step 3 — Configure your `.env` file (in Termux)

1. Stop the server (`Ctrl+C`).
2. Copy the sample:
   ```
   cp .env.sample .env
   ```
3. Open it:
   ```
   nano .env
   ```
4. Clear it: `Alt+A`, `Alt+/`, `Ctrl+K`.
5. Paste this, filled in with your keys from Step 1.6 and your timezone (e.g. `Asia/Kolkata`):
   ```
   WUZAPI_ADMIN_TOKEN=your_admin_token_here
   WUZAPI_GLOBAL_ENCRYPTION_KEY=your_encryption_key_here
   WUZAPI_GLOBAL_HMAC_KEY=your_hmac_key_here

   SESSION_DEVICE_NAME=macOS
   WUZAPI_PORT=8080

   TZ=your_timezone_here

   WEBHOOK_FORMAT=json

   WEBHOOK_RETRY_ENABLED=true
   WEBHOOK_RETRY_COUNT=1
   WEBHOOK_RETRY_DELAY_SECONDS=30
   ```
   What each setting does: [Configuration](configuration.md).
6. Save with `Ctrl+O`, `Enter`, then exit with `Ctrl+X`.
7. Start the server and leave it running:
   ```
   ./wuzapi
   ```

## Step 4 — Finish setup in Tasker

1. Go to **Tasker → App Info → Permissions → Additional Permissions** and enable **Run commands in Termux environment**.
2. Run **WUZ - Project Setup** to register the webhook.

🎉 Done. Next: [User Guide](user-guide.md) or [Event Preferences](event-preferences.md).

Android may kill Termux in the background: see [Background reliability](troubleshooting.md#background-reliability).
