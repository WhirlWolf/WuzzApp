# WuzzApp
WhatsApp mod-level features, Tasker-level control. React to real-time events and automate anything.

Uses [WuzAPI](https://github.com/asternic/wuzapi).

## Disclaimer
- This project is **not affiliated with WhatsApp**.
- You are responsible for how you use it.
- Use responsibly to avoid account issues.
- Proceed at your own risk.

## Prerequisites
- [Termux](https://f-droid.org/repo/com.termux_1022.apk)
- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm)


## Setup Instructions
> [!NOTE]
> Internet consumption ~ 500MB

### Step 1: Termux

1. [Download and install Termux](https://f-droid.org/repo/com.termux_1022.apk)
2. Open Termux and allow notification permission if asked
3. Update packages and enter `y` when prompted
    ```
    apt update && apt upgrade
    ```
4. Install dependencies
    ```
    apt install git golang sqlite -y
    ```
5. Allow external apps to interact with Termux
    ```
    sed -i '/^#\s*allow-external-apps\s*=\s*true/s/^#\s*//' ~/.termux/termux.properties
    termux-reload-settings
    ```
6. Grant storage permission and allow when prompted
    ```
    termux-setup-storage
    ```
7. Clone and build the server
    ```
    git clone https://github.com/asternic/wuzapi
    cd wuzapi
    go build
    ./wuzapi
    ```
  >[!IMPORTANT]
  > Copy and save the generated **admin token**, **global encryption key**, and **global HMAC key** — you'll need them later.
    
    Press `Ctrl+C` to stop the server once done

8. Create a user — `id`, `name`, and `token` can be anything you want. Save the token for later
    ```
    sqlite3 dbdata/users.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
    ```
9. Start the server
    ```
    ./wuzapi
    ```
10. [Import project in Tasker](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)

### Step 2: Link Device

1. Open http://localhost:8080/dashboard/ in your browser
2. Enter your token and log in
3. Tap **Connect**

 > [!NOTE]
 > This step may not be required in newer versions

4. In Tasker, run the **WUZ - LinkWithPhoneNumber** task — a code will be copied to your clipboard
5. In WhatsApp, go to **Link Device → Link with Phone Number** and enter the code from your clipboard

  > [!TIP]
  > WhatsApp may notify you directly to connect — follow the prompt if it appears

### Step 3: Termux
- Ctrl+c to kill session
- Prepare .env file
	- add token, global encryption key, global hmac key and your timezone (e.g. Asia/Kolkata) in following text and copy it
> Admin token, global encryption key and global hmac key can be any 32 characters string but it is recommended to use previously saved (generated) credentials.
```	
WUZAPI_ADMIN_TOKEN=admin_token_generated_earlier_here
WUZAPI_GLOBAL_ENCRYPTION_KEY=global_encryption_key_generated_earlier_here
WUZAPI_GLOBAL_HMAC_KEY=global_hmac_key_generated_earlier_here

SESSION_DEVICE_NAME=macOS

WUZAPI_PORT=8080

TZ=timezone_here

WEBHOOK_FORMAT=json

WEBHOOK_RETRY_ENABLED=true
WEBHOOK_RETRY_COUNT=1
WEBHOOK_RETRY_DELAY_SECONDS=30
```
- Run `cp .env.sample .env`
- Run `nano .env`
	- do Alt + A   (start select)
		then Alt + /   (go to end)
		then Ctrl + K  (cut all)
	- paste text copied in previous step (.env)
	- ctrl+o to save > enter, ctrl+x to exit
- Run `./wuzapi`

### Step 4: Tasker
- Go to app info > permissions > additional permissions > run commands in termux environment 
- Run task **WUZ - Connect**
- Run task **WUZ - Setup**

Setup Complete!


## Updating
### Library (Termux)
```
pkill ./wuzapi

cd wuzapi

git pull

go get -u go.mau.fi/whatsmeow@latest
go mod tidy

go build
./wuzapi

```
### Project (Tasker)
[Import](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp) from Taskernet > run setup task (existing setup will be erased).

## Features

### Real-Time Alerts
- **Status alerts** — get notified when a contact posts or updates their status
- **Read receipt alerts** — know the moment your message is seen
- **Typing alerts** — trigger actions as soon as someone starts typing

### Contacts
- **Profile picture sync** — automatically pull high-quality profile pictures into your device contacts
- **WhatsApp user check** — verify whether a phone number is on WhatsApp

### Messaging
- **Send text messages** — send WhatsApp messages directly
- **Go to Chat** — jump straight into a conversation from any trigger or task

### Automation
- **Embedded triggers** — fire tasks directly from within alert
- **Automatic triggers** — let WuzzApp kick off tasks on its own based on WhatsApp events
- **Multiple alert types** — choose how and when you get notified

### General
- **[Updater](https://github.com/WhirlWolf/Updater) support** — stay up to date automatically

## Privacy
- WuzzApp does not collect or send your personal data anywhere by default.
- In short:  **Your data stays yours unless you choose otherwise.**

## Safety
- Avoid creating aggressive or spam-like automations.
- Don’t expose your server to the public internet without protection.
- Never share your:
  - Encryption keys
  - Token
  - Webhook URL
  - `.env` file
- If something gets exposed, **regenerate it immediately**.

> Some actions can trigger infinite loops if misused. For e.g., using **Send Text Message** inside a **Message Delivered** trigger will cause it to fire itself repeatedly. Use with caution until built-in anti-loop protection is added in a future update.

## FAQ

<details>
<summary><strong>Why is WuzzApp not working reliably in the background?</strong></summary>

Ensure both **Termux** and **Tasker** are allowed to run in the background and are not restricted by battery optimization settings.

</details>

<details>
<summary><strong>Some automations are not triggering or are delayed. What should I do?</strong></summary>

Increase the maximum task queue in Tasker:

**Tasker → Preferences → Action → Max Tasks Queued**

Adjust based on your WhatsApp activity.

</details>

<details>
<summary><strong>Why does my WhatsApp show me as “online” frequently?</strong></summary>

Your status may appear “online” as long as you are connected to the server.

Change this in:

**WhatsApp → Settings → Privacy → Last Seen & Online**

</details>

<details>
<summary><strong>What happens when I tap on a “composing” flash alert?</strong></summary>

It opens the chat of the person who is typing.

</details>

<details>
<summary><strong>Termux/server stops running in the background. How to fix it?</strong></summary>

Tap **“Acquire wakelock”** in the Termux notification to improve background reliability.

</details>

<details>
<summary><strong>How can I enable colored logs?</strong></summary>

Stop the server, then restart it using:

```bash
./wuzapi -logtype json
```
</details>
<details>
<summary><strong>What should I do if I get stuck in a loop?</strong></summary>

Open Tasker's running tasks notification and stop the task from there. If that doesn't work, force stop Tasker or reboot your device.

</details>

## Contributing
Found a bug or have a feature request?
[Open an issue](https://github.com/WhirlWolf/WuzzApp/issues)

---

<a href="https://ko-fi.com/whirlwolf" target="_blank">
  <img src="https://cdn.ko-fi.com/cdn/kofi2.png?v=3" alt="Buy Me a Coffee" width="150"/>
</a>
