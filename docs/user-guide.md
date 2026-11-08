# User Guide

Each feature is a task in the Tasker project. Task names (may change before v1) are listed so you can find them or call them from your own profiles.

## Real-time alerts

WuzzApp listens for WhatsApp events through wuzapi's webhook and can alert you instantly.

| Event | Meaning |
|---|---|
| Connected / Disconnected / Logged Out | Session connection changed |
| Status Posted / Deleted | A contact posted or removed a status |
| Message Delivered | Your message was delivered |
| Message Read | Your message was read (blue ticks) |
| Message Received / Sent / Deleted | A message came in, went out, or was deleted |
| Chat Presence Composing / Paused | Someone started or stopped typing to you |

Each event is built from **cards** you stack and configure: Flash, Notify, FAO, Run Task, Say, Vibrate and more. Only a basic alert is set up by default. Full reference: [Event Preferences](event-preferences.md).

## Contacts

- **WUZ - Sync Profile Pictures To Contacts:** copies WhatsApp profile pictures into your phone contacts.
- **WUZ - CheckUser:** checks if a number has WhatsApp.

## Messaging

- **Send Text Message**, **Send Image Message**, **Send Location:** send straight from Tasker.
- **Go To Chat:** opens a chat in WhatsApp. Not wired up by default. Add it as a **Run Task** under an alert's **Tap** interaction ([Interactions](event-preferences.md#interactions)).

These are plain Tasker tasks. Call them from your own profiles with a number and message.

## Automation

- **Action cards** (Run Task, Vibrate, Set/Toggle Variable, Send Command, etc.) run instantly when an event fires. Useful for logging or auto-replies, but the easiest way to cause a spam loop ([Safety](safety-and-privacy.md)).
- **Interactions** (Tap, Hold, Multi-tap, Swipe, notification buttons) run cards only when you engage with an alert.
- **Conditions** limit any card or interaction by chat/group, sender, time, days or foreground app ([Conditions](event-preferences.md#conditions)).

## Connection

- **WUZ - Connect:** connects Tasker to wuzapi and starts listening.
- **WUZ - Disconnect:** stops the session but keeps the device linked.
- **WUZ - Logout:** unlinks the device. Relink with **WUZ - LinkWithPhoneNumber** or **WUZ - Project Setup**.
- **WUZ - LinkWithPhoneNumber:** generates the linking code.

## Setup and maintenance

- **WUZ - Project Setup:** registers the webhook after install or update.
- **WUZ - Project Info:** shows version info.
- **WUZ - Manage Subscribed Events:** choose which events wuzapi sends. Fewer events means less background work.
- **WUZ - Manage Preset Tasks:** edit the task suggestions in Event Preferences.

## Updater

[Updater](https://github.com/WhirlWolf/Updater) checks for new project versions. Manual steps: [Updating](updating.md).
