# Updating

The **wuzapi library** (Termux) and the **Tasker project** update separately.

## Update wuzapi (Termux)

```
pkill ./wuzapi
cd wuzapi
git pull
go get -u go.mau.fi/whatsmeow@latest
go mod tidy
go build
./wuzapi
```

Your `.env` and linked session are untouched.

## Update the project (Tasker)

1. [Import the latest version from TaskerNet](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)
2. Run **WUZ - Project Setup** to re-register the webhook.

> **Warning:** Importing **erases your edits inside the WuzzApp project** (task tweaks, added profiles). Your wuzapi link is safe. Event Preferences are stored in a variable and may reset, so back them up below. To keep your edits, export the project first (Tasker → long-press project → Export).

## Back up Event Preferences

1. In Tasker, open the project properties → variables and find the Event Preferences variable.
2. Long-press it and save its value to a note or file.
3. After updating, set the variable to the saved value and reopen Event Preferences to check it loaded.

Only works between matching WuzzApp versions.

## If something breaks after updating

- Updating only one side can cause mismatches. Check [issues](https://github.com/WhirlWolf/WuzzApp/issues).
- Webhook behavior changed? Re-run **WUZ - Project Setup**.
- Event Preferences empty or broken? They may predate a redesign. Rebuild them using [Event Preferences](event-preferences.md).

## Update checks

[Updater](https://github.com/WhirlWolf/Updater) can notify you of new project versions.
