# Getting Started

This guide walks you through installing WuzzApp from scratch.

**Time required:** ~20–30 minutes
**Data required:** ~500MB (mostly the Go toolchain and wuzapi build)

## What you're installing

WuzzApp has three moving parts:

| Part                     | What it is                                                                      | Where it runs                              |
| ------------------------ | ------------------------------------------------------------------------------- | ------------------------------------------ |
| **wuzapi**               | The WhatsApp connection server (open source, by asternic)                       | Inside Termux, on your phone               |
| **Tasker project**       | The automation brain — reacts to WhatsApp events, sends alerts, runs your tasks | Tasker app, on your phone                  |
| **Event Preferences UI** | A settings screen for configuring alerts/triggers per event                     | Opens from within WuzzApp (Tasker project) |

Both wuzapi and the Tasker project run **locally on your device** — nothing is sent to a remote server unless you configure a webhook yourself.

## Prerequisites

- [Termux](https://f-droid.org/repo/com.termux_1022.apk) — install from F-Droid, **not** the Play Store version
- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm) — paid app, required

## Step 1 — Install and build wuzapi (in Termux)

1. Install Termux from the link above and open it. Allow the notification permission if prompted — this keeps the server alive in the background.

2. Update Termux's packages:
   ```
   apt update && apt upgrade
   ```
   Press `y` when prompted to confirm.

3. Install the build dependencies:
   ```
   apt install git golang sqlite -y
   ```

4. Allow Tasker to send commands into Termux:
   ```
   sed -i '/^#\s*allow-external-apps\s*=\s*true/s/^#\s*//' ~/.termux/termux.properties
   termux-reload-settings
   ```
   Without this, Tasker's **Run Shell** actions (used in the project) won't be able to reach Termux.

5. Grant storage access when prompted:
   ```
   termux-setup-storage
   ```

6. Clone and build wuzapi:
   ```
   git clone https://github.com/asternic/wuzapi
   cd wuzapi
   go build
   ./wuzapi
   ```
   The build can take a couple of minutes on a phone — this is normal.

   > **Important:** On first run, ignore warns, wuzapi prints an **admin token**, **global encryption key**, and **global HMAC key**. Copy these for convinience — you can use these into your `.env` file in Step 3. Treat them like passwords — see [Safety & Privacy](safety-and-privacy.md#securing-your-setup) for handling and what to do if they're ever exposed.

   Once it's running, press `Ctrl+C` to stop it — you're not ready to leave it running yet.

7. Create a WhatsApp user in wuzapi's database. `id`, `name`, and `token` can be any values you like (i.e. whirlwolf, tangerine, etc) — just remember them:
   ```
   sqlite3 dbdata/users.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
   ```

8. Start the server again:
   ```
   ./wuzapi
   ```

9. Import the Tasker project: [Import WuzzApp from TaskerNet](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)

## Step 2 — Link your WhatsApp

1. With wuzapi still running, open `http://localhost:8080/dashboard/` in your phone's browser.
2. Log in using the **token** you created in Step 1.8 (not the admin token).
3. Tap **Connect**.

> These first three steps may not be necessary on newer wuzapi versions — if opening the dashboard shows a **Connected** status without you tapping anything, the session auto-connected and you can skip ahead. If the dashboard shows a login prompt instead, follow steps 1–3 as written.

4. In Tasker, run the **WUZ - Project Setup** task. It copies a linking code to your clipboard.
5. In WhatsApp, go to **Linked Devices → Link with Phone Number** and paste in the code.

> WhatsApp sometimes prompts you directly to connect a linked device — if you see that prompt, you can follow it instead.

## Step 3 — Configure your `.env` file (in Termux)

1. Stop the server (`Ctrl+C`).
2. Copy the sample env file:
   ```
   cp .env.sample .env
   ```
3. Fill in this block with the admin token, encryption key, and HMAC key from Step 1.6, and your timezone (e.g. `Asia/Kolkata`):
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
   See the [Configuration Reference](configuration.md) for what each of these does.

4. Open the file for editing:
   ```
   nano .env
   ```
5. Clear the existing contents:
   - `Alt+A` — start selection
   - `Alt+/` — jump to end
   - `Ctrl+K` — cut everything
6. Paste in your filled-out block from step 3.
7. Save and exit:
   - `Ctrl+O` → `Enter` to save
   - `Ctrl+X` to exit
8. Start the server:
   ```
   ./wuzapi
   ```
   Leave it running — this is the server WuzzApp talks to.

## Step 4 — Finish setup in Tasker

1. Go to **Tasker → App Info → Permissions → Additional Permissions** and enable **Run commands in Termux environment**. This is required for any Run Shell action to work.
2. Run the **WUZ - Project Setup** task. This registers the webhook and prepares the project.

🎉 **You're done.** Head to the [User Guide](user-guide.md) to see what to do next, or [Event Preferences](event-preferences.md) to configure alerts.

## Keeping the server alive

Termux will be killed by Android's battery optimizer unless you exempt it. See [Troubleshooting → Background reliability](troubleshooting.md#background-reliability) for device-specific steps.
