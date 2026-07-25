# Updating

WuzzApp has two independent pieces that update separately: the **wuzapi library** (Termux) and the **Tasker project**.

## Updating the library (Termux)

```
pkill ./wuzapi
cd wuzapi
git pull
go get -u go.mau.fi/whatsmeow@latest
go mod tidy
go build
./wuzapi
```

This pulls the latest wuzapi source, updates the underlying WhatsApp protocol library (`whatsmeow`), and rebuilds the binary. Your `.env` file and linked session are untouched.

## Updating the project (Tasker)

1. [Import the latest version from TaskerNet](https://taskernet.com/shares/?user=AS35m8m8L9YzBV3qbzaAAqHiSYXYBbD3QfZ7hr0hRK4ojOFTCrjWh2CScbjMw4NaudRi1zKKzq85&id=Project%3AWuzzApp)
2. Run the **WUZ - Project Setup** task afterward to re-register the webhook and finish setup.

> **Warning:** Re-importing the project **erases your existing Tasker-side setup** — this includes anything you customized directly inside the WuzzApp project (task tweaks, added profiles inside it). Your **Event Preferences** (stored as a Tasker variable) and your wuzapi session/link are *not* affected, since those live outside the project file.
>
> If you've heavily customized the project itself, consider backing it up (Tasker → long-press project → Export) before updating.

## Backing up and restoring Event Preferences

Event Preferences are stored as a Tasker project variable,  — re-importing the project would reset them:

1. In Tasker, go to the project properties > variables list and find the Event Preferences variable (check [Event Preferences](event-preferences.md) or [User Guide](user-guide.md) for the current variable name if you're unsure).
2. Long-press it save its value (e.g. copy it to a note or file).
3. To restore, set the same variable to that saved value on the new install, then reopen the Event Preferences screen to confirm it loaded correctly.

Note that this only works between matching WuzzApp versions.

## Version compatibility

wuzapi and the Tasker project update independently, and their versions aren't guaranteed to be interchangeable:

- Check the [WuzzApp GitHub releases/issues](https://github.com/WhirlWolf/WuzzApp/issues) if you update one side and something stops working — a mismatch is a common cause.
- If you update wuzapi and the dashboard/webhook behavior changes unexpectedly, try re-running **WUZ - Project Setup** to re-register the webhook before assuming something else is wrong.
- If you update the Tasker project and Event Preferences look empty or broken afterward, your saved preferences may predate a schema change — see the [changelog](event-preferences-changelog.md) for whether a redesign happened between your old and new versions.

## Automatic update checks

WuzzApp supports the [Updater](https://github.com/WhirlWolf/Updater) plugin, which can notify you when a new project version is available so you don't have to check manually.
