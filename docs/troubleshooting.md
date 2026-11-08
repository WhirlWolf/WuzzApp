# Troubleshooting

## Background reliability

**Symptom:** works while you use the phone, stops in your pocket.

Android is killing Termux or Tasker.

1. Exclude **Termux** and **Tasker** from battery optimization.
2. Follow your phone's steps at [dontkillmyapp.com](https://dontkillmyapp.com).
3. In Termux's notification, tap **Acquire wakelock**.

## Automations not firing or firing late

Tasker caps queued tasks, and busy chats can back up. Raise **Tasker → Preferences → Action → Max Tasks Queued**.

## WhatsApp shows me "online" a lot

Expected while connected. Change it in **WhatsApp → Settings → Privacy → Last Seen & Online**.

## Tapping an alert doesn't open the chat

Nothing is wired to taps by default. See [Event Preferences → Interactions](event-preferences.md#interactions).

## Stuck in a loop of alerts or messages

An automatic trigger is re-triggering itself. For example, **Send Text Message** on **Message Delivered** sends, which delivers, which sends again.

**Stop it:**
1. Stop the task from Tasker's running-tasks notification.
2. Otherwise force-stop Tasker in Android settings.
3. Last resort: reboot.

**Prevent it:** [Safety & Privacy](safety-and-privacy.md#avoiding-a-whatsapp-ban).

## Server won't start / port in use

Stop the other process, or change `WUZAPI_PORT` in `.env` ([Configuration](configuration.md)) and restart.

## Colored / structured logs

```
./wuzapi -logtype json
```

## Linking fails or times out

- Make sure the dashboard opens at `http://localhost:8080/dashboard/` first.
- Codes expire fast. Re-run the task for a fresh one.
- After a **Logout** you must relink. This is normal.

## Uninstall or reset

1. **Unlink first** while the server runs: run **WUZ - Logout** or remove the device in WhatsApp's **Linked Devices**.
2. Stop the server (`Ctrl+C` or `pkill ./wuzapi`).
3. Delete the `wuzapi` folder with a file manager. Avoid `rm -rf` in Termux; a typo can wipe your device. This removes the server, `.env` and `dbdata/`.
4. In Tasker, long-press the **WuzzApp** project and delete it with contents. This also removes your Event Preferences.
5. Optionally uninstall Termux and Tasker.

**To start over instead:** run **WUZ - Logout**, stop the server, delete `dbdata/main.db` (clears all users and sessions), then redo [Getting Started](getting-started.md) from Step 1.7.

## Still stuck?

[Open an issue](https://github.com/WhirlWolf/WuzzApp/issues) with:
- The step you're on
- The exact error
- Termux and Tasker versions
- Project version and build number
