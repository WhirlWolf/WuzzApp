# User Guide

A walkthrough of everything WuzzApp can do. Each feature maps to a task in the Tasker project — the exact task name (may change until v1) is included so you can find it or wire it into your own Tasker profiles.

## Real-Time Alerts

WuzzApp listens for WhatsApp events (via wuzapi's webhook) and can show an on-screen alert the moment they happen.

| Event | What it means |
|---|---|
| Connected / Disconnected / Logged Out | Your WhatsApp session's connection state changed |
| Status Posted / Status Deleted | A contact posted or removed a WhatsApp status |
| Message Delivered | A message you sent was delivered |
| Message Read | A message you sent was read (blue ticks) |
| Message Received / Sent / Deleted | A message came in, went out, or was deleted |
| Chat Presence Composing / Paused | Someone started or stopped typing to you |

Each event is built from **cards** — independent alert or action blocks you stack and configure yourself (Flash, Notify, FAO overlay, Run Task, Say, Vibrate, and more), with taps/holds/swipes able to trigger further cards. This gives you far more control than a fixed "one alert + one task" pair, but it also means nothing beyond a basic alert is wired up until you configure it.

The full reference — every card type, field, and condition — lives in **[Event Preferences](event-preferences.md)**.

## Contacts

- **Profile picture sync** (task: **WUZ - Sync Profile Pictures To Contacts**) — pulls high-resolution WhatsApp profile pictures into your phone's native contacts, so they show up in your dialer and messaging apps.
- **WhatsApp user check** (task: **WUZ - CheckUser**) — checks whether a given phone number has WhatsApp, useful before sending a message or building your own automations.

## Messaging

- **Send Text Message** — send a WhatsApp message to any number directly from a Tasker task, no need to open WhatsApp.
- **Send Image Message** — same, with an image attached.
- **Send Location** — share a location to a chat.
- **Go To Chat** — jumps straight into a specific conversation in WhatsApp. Not wired up by default anymore — attach it as a **Run Task** action under an alert card's **Tap** interaction in [Event Preferences](event-preferences.md#interactions) if you want that behavior.

All of these are plain Tasker tasks — you can call them from your own profiles, pass in a phone number and message, and use them as building blocks in bigger automations (reminders, alerts forwarded from other apps, etc.).

## Automation

- **Action cards** (Run Task, Vibrate, Set/Toggle Variable, Send Command, etc) run automatically the instant an event fires — no interaction needed. Useful for things like logging or auto-replying, but see the [Safety](safety-and-privacy.md) page — these are the main way to accidentally create a spam loop.
- **Interactions** (Tap, Hold, Multi-tap, Swipe, or notification buttons) let an alert card run a chain of action cards only when you actually engage with it — nothing happens unless you set it up.
- **Conditions** — every section, card and interaction can be restricted to only fire when certain conditions are true: chat vs. group, specific senders (or "me"), time of day, specific days, or specific foreground apps. See [Event Preferences](event-preferences.md#conditions).

## Connection Management

- **WUZ - Connect** — connects Tasker to your running wuzapi server and starts listening for events.
- **WUZ - Disconnect** — stops the WhatsApp session without logging out (keeps your linked device).
- **WUZ - Logout** — fully unlinks the device from WhatsApp; you'll need to relink with **WUZ - LinkWithPhoneNumber** or **WUZ - Project Setup**.
- **WUZ - LinkWithPhoneNumber** — generates the linking code used during initial setup or after a logout.

## Setup & Maintenance Tasks

- **WUZ - Project Setup** — registers the webhook with wuzapi and prepares the project after install or update.
- **WUZ - Project Info** — shows current project/version info.
- **WUZ - Manage Subscribed Events** — choose which WhatsApp event types wuzapi should actually send to WuzzApp. Unsubscribing from events you don't use reduces background work.
- **WUZ - Manage Preset Tasks** — manage the list of tasks that show up as suggestions in the Event Preferences task picker.

## Updater

WuzzApp supports the [Updater](https://github.com/WhirlWolf/Updater) for automatic update checks on the project. See [Updating](updating.md) for manual update steps either way.
