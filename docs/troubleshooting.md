# Troubleshooting

## Background reliability

**Symptom:** WuzzApp works fine when you're using your phone, but alerts/triggers stop after a while in your pocket.

This is almost always Android killing Termux or Tasker to save battery.

1. Make sure both **Termux** and **Tasker** are excluded from battery optimization and allowed to run in the background.
2. Steps vary a lot by phone manufacturer (Xiaomi, Samsung, OnePlus, etc. all have their own aggressive battery managers) — visit [dontkillmyapp.com](https://dontkillmyapp.com) and follow the instructions for your specific device.
3. In Termux's persistent notification, tap **Acquire wakelock** — this keeps the CPU from sleeping while the server needs to run.

## Automations not firing, or firing late

Tasker has a cap on how many tasks can be queued at once. If you have a busy chat, events can back up.

**Tasker → Preferences → Action → Max Tasks Queued** — increase this value based on how much WhatsApp activity you get.

## WhatsApp shows you as "online" a lot

This is expected — WhatsApp shows you as online for as long as your device is actively connected to the server (which needs to happen for WuzzApp to receive events). If this bothers you, change:

**WhatsApp → Settings → Privacy → Last Seen & Online**

## Tapping a "composing" alert doesn't open the chat

Nothing is wired to alert taps by default anymore — this is expected. See [Event Preferences → Interactions](event-preferences.md#interactions) to wire it up yourself.

## Stuck in a loop of alerts or messages

This usually means an **automatic** trigger is re-triggering itself — e.g. a "Send Text Message" task wired to a "Message Delivered" trigger will keep sending, since each send delivers, which fires the trigger again.

**To stop it immediately:**
1. Open Tasker's running-tasks notification and stop the offending task from there.
2. If that doesn't work, force-stop Tasker from Android's app settings.
3. As a last resort, reboot the device.

**To prevent it happening again:** see [Safety & Privacy → Avoiding a WhatsApp ban](safety-and-privacy.md#avoiding-a-whatsapp-ban), which covers the same loop risk and how to design around it.

## Server won't start / port already in use

If `./wuzapi` fails to start, another process may already be using the port (default `8080`). Either stop that process, or change `WUZAPI_PORT` in `.env` (see [Configuration](configuration.md)) and restart.

## Enabling colored / structured logs

Stop the server and restart with:

```
./wuzapi -logtype json
```

## Linking fails or times out

- Make sure the server is running and reachable at `http://localhost:8080/dashboard/` before running **WUZ - LinkWithPhoneNumber**/**WUZ - Project Setup**.
- Codes expire quickly — if WhatsApp rejects the code, re-run the task to generate a fresh one.
- If you're re-linking after a **Logout**, this is expected — logout fully unlinks the device.

## Uninstalling / fully resetting WuzzApp

If you want to remove WuzzApp entirely (e.g. before a device change, or to start over cleanly):

1. **Unlink WhatsApp first**, while the server is still running — run the **WUZ - Logout** task, or unlink the device manually from WhatsApp's **Linked Devices** screen. Doing this before removing anything else avoids leaving a stale linked device in your WhatsApp account.
2. **Regenerate/revoke tokens** — since `WUZAPI_ADMIN_TOKEN`, the encryption key, and the HMAC key won't be usable once you delete the server, there's nothing further to revoke, but stop the server (`Ctrl+C` or `pkill ./wuzapi`) before deleting files so nothing writes to the database mid-delete.
3. **Remove the wuzapi install (SKIP THIS STEP because of dangerous command, an accidental tap will literally delete everything in your device including your OS)** — in Termux: `rm -rf ~/wuzapi` (or wherever you cloned it) deletes the server, its `.env` file, and its local `dbdata/users.db`.
4. **Remove the Tasker project** — in Tasker, long-press the **WuzzApp** project and delete (with contents) it. This also removes your Event Preferences, since they're stored as a Tasker variable scoped to the project.
5. **Uninstall Termux/Tasker** (optional) — only needed if you're not using them for anything else.

If you only want to **start over** rather than fully uninstall, you can skip steps 3–5 and instead: run **WUZ - Logout**, delete `dbdata/users.db` to clear old users, restart the server, and follow [Getting Started](getting-started.md) from Step 1.7 (creating a user) onward.

## Still stuck?

[Open an issue](https://github.com/WhirlWolf/WuzzApp/issues) with:
- What step you're on
- The exact error/behavior
- Your Termux and Tasker versions
- Project version and build number
