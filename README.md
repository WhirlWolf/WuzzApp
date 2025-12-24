# WuzzApp
Control WhatsApp using Tasker.

## Prerequisites
- [Termux](https://f-droid.org/repo/com.termux_1022.apk)
- [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm)

## Setup Instructions
#### Termux
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
	- Copy and save generated admin token (32 characters; required later)
	- ctrl+c to stop server
- Run
  ```
  sqlite3 dbdata/users.db "insert into users ('id','name','token') values ('whirlwolf','WhirlWolf','whirlwolf')"
  ```
	- values can be anything you want
	- save token (required later)
- Run `./wuzapi` 

- [Import project](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)

#### Link device
 - goto http://localhost:8080/dashboard/
	- Enter token > login
	- tap on connect
- TASKER: run task **WUZ - LinkWithPhoneNumber**
	- a code will be copied to clipboard
- Goto whatsapp > link device > link with phone number (usually whatsapp notifies directly to connect)
	- enter code from clipboard

#### Termux
- ctrl+c to kill session
- prepare .env file
	- add token and your timezone (e.g. Asia/Kolkata) in following text and copy it
```	
WUZAPI_ADMIN_TOKEN=admin_token_generated_earlier_here

SESSION_DEVICE_NAME=WuzAPI

WUZAPI_PORT=8080

TZ=timezone_here

WEBHOOK_FORMAT=json
```
- run `mv .env.sample .env`
- run `nano .env`
	- do Alt + A   (start select)
		then Alt + /   (go to end)
		then Ctrl + K  (cut all)
	- paste text copied in previous step (.env)
	- ctrl+o to save > enter, ctrl+x to exit
- run `./wuzapi`

#### Tasker
- Goto app info > permissions > additional permissions > run commands in termux environment 
- run task **WUZ - Connect**
- run task **WUZ - Setup**

> Make sure termux & tasker are allowed to run in background

> Increase maximum tasks queued in Tasker > preferences > action (depending on your whatsapp activity)

ALL DONE!

Internet consumption (may vary): ~500mb
