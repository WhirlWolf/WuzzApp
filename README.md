# WuzzApp
Control WhatsApp using Tasker.

Uses [WuzAPI](https://github.com/asternic/wuzapi)

## Warning
This project is for educational and experimental purposes only. Misuse may violate WhatsApp’s terms of service and could result in account restrictions or bans. Use at your own risk; the author is not responsible for any damage, data loss, or account issues arising from its use.

## Prerequisites
- [Termux](https://f-droid.org/repo/com.termux_1022.apk)
- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm)

## Setup Instructions
### Termux
- Install from fdroid
- Open and allow notification permission if asked
- Run `apt update && apt upgrade`
	- follow with 'y'
- Run `apt install git golang sqlite -y`
- Run
```
sed -i '/^#\s*allow-external-apps\s*=\s*true/s/^#\s*//' ~/.termux/termux.properties
termux-reload-settings
```
- Run `termux-setup-storage` > give permission
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

### Link device
- Go to http://localhost:8080/dashboard/
	- Enter token > login
	- tap on connect
> Above step may not be required in newer versions
- TASKER: run task **WUZ - LinkWithPhoneNumber**
	- a code will be copied to clipboard
- Go to whatsapp > link device > link with phone number (usually whatsapp notifies directly to connect)
	- enter code from clipboard

### Termux
- Ctrl+c to kill session
- Prepare .env file
	- add token, global encryption key, global hmac key and your timezone (e.g. Asia/Kolkata) in following text and copy it
> Admin token, global encryption key and global hmac key can be any 32 characters string if you know what you are doing! It is recommended to use previously saved (generated) credentials.
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

### Tasker
- Go to app info > permissions > additional permissions > run commands in termux environment 
- Run task **WUZ - Connect**
- Run task **WUZ - Setup**

ALL DONE!

Internet required: 500mb (may vary)

## Updating
### Termux
- Run
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
- Sync high quality profile pictures with device contacts
- Send text message
- Check if a contact is a whatsapp user
- [Updater](https://t.me/android_automation/163) support

## Tips
- Make sure termux & tasker are allowed to run in background
- Increase maximum tasks queued in tasker > preferences > action (depending on your whatsapp activity)
- Your status will be shown "online" as long as you are connected to the server, you may want to change this in whatsapp > settings > privacy > last seen and online
- Tap on composing flash alert to go to composer's chat
- Tap "Aquire wakelock" in termux notification if termux or server doesn't run reliably in background
- To get colored logs, kill the server, start new server using `./wuzapi -logtype json `
