# WuzzApp
Control WhatsApp using Tasker.

Uses [WuzAPI](https://github.com/asternic/wuzapi)

## Warning
>This project is created for educational and experimental purposes only and is provided “as is.”

>It is not affiliated with WhatsApp. Any use that breaches WhatsApp’s Terms of Service is solely the user’s responsibility and may result in account restrictions or bans. The author expressly disclaims all liability for account bans, data loss, or any consequential damages.

>Proceed at your own risk.

## Prerequisites
- [Termux](https://f-droid.org/repo/com.termux_1022.apk)
- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm)

## Setup Instructions
### Step1: Termux
- [Download](https://f-droid.org/repo/com.termux_1022.apk) and install
- Open and allow notification permission if asked
- Run `apt update && apt upgrade` and follow with 'y'
- Run `apt install git golang sqlite -y`
- Run
```
sed -i '/^#\s*allow-external-apps\s*=\s*true/s/^#\s*//' ~/.termux/termux.properties
termux-reload-settings
```
- Run `termux-setup-storage` > allow permission
- Run `git clone https://github.com/asternic/wuzapi`
- Run `cd wuzapi`
- Run `go build`
- Run `./wuzapi`
	- Copy and save generated admin token, global encryption key and global hmac key (32 characters; required later)
	- ctrl+c to stop server
- Run
  ```
  sqlite3 dbdata/users.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
  ```
	- values can be anything you want
	- save token (required later)
- Run `./wuzapi` 

- [Import project](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)

### Step 2: Link device
- Go to http://localhost:8080/dashboard/
	- Enter token > login
	- tap on connect
> Above step may not be required in newer versions
- TASKER: run task **WUZ - LinkWithPhoneNumber**
	- a code will be copied to clipboard
- Go to whatsapp > link device > link with phone number (usually whatsapp notifies directly to connect)
	- enter code from clipboard

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

ALL DONE!

> Internet consumption ~ 500mb

## Updating
- Updating library
```
pkill ./wuzapi

cd wuzapi

git pull

go get -u go.mau.fi/whatsmeow@latest
go mod tidy

go build
./wuzapi

```

## Features
- Get status alerts
- Get read receipt alerts
- Get typing alerts
- Sync high quality profile pictures to device contacts
- Send text message
- Check if a contact is a whatsapp user
- [Updater](https://github.com/WhirlWolf/Updater) support
- Alert types
- Embedded triggers
- Automatic triggers
- Go to Chat

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
