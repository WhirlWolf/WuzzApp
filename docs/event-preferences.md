# Event Preferences

The **Event Preferences** scene (open via the **WUZ - Event Preferences** task) is where you configure exactly how WuzzApp reacts to each WhatsApp event.

> This screen was significantly redesigned from earlier versions. If you used WuzzApp before, the old "one alert + one task per event" model has been replaced by a **card-based system**: each event can now have any number of independent cards (alerts or actions), and each alert card can have its own interactions (tap, hold, swipe, buttons) that fire further cards. This page documents the current version, as shipped inside the WuzzApp project itself.
>
> **Upgrading from an older version?** Your saved Event Preferences won't carry over automatically, and old defaults (like tap-to-open-chat) won't be wired up anymore. See the event preferences changelog for exactly what changed and what you'll need to rebuild.

## Events

12 event types are available, each as its own collapsible section:

**Connection**
- Connected
- Disconnected
- Logged Out

**Status**
- Status Posted
- Status Deleted

**Messages**
- Message Delivered
- Message Read
- Message Received
- Message Sent
- Message Deleted

**Presence**
- Chat Presence Composing
- Chat Presence Paused

Every event starts with one default **Flash** card (see below) and no interactions — you build up from there.

## Cards

A **card** is a single unit of behavior attached to an event. You can add as many as you like per event, reorder them by dragging and duplicate by long tapping on drag handle. Cards come in two families:

### Alert cards

These show something to you, and can have **interactions** — ways of reacting to them that trigger further cards.

| Card                              | What it shows                                                                      | Interactions available       |
| --------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------- |
| **Flash**                         | A temporary on-screen overlay, colored per-sender by default                       | Tap                          |
| **Notify**                        | A standard Android notification, with up to 3 buttons                              | Button 1, Button 2, Button 3 |
| **FAO** (Floating Action Overlay) | A persistent, positioned scenev2 bubble/badge that stays on screen until dismissed | Tap, Hold, Multi-tap, Swipe  |

### Action cards

These run automatically the moment the event fires — no interaction needed. They're also what you attach to an alert card's interactions (e.g. "on tap, Run Task").

| Card                | What it does                                                                                                                                                         |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Say**             | Speaks the text out loud via TTS (voice, pitch, speed configurable)                                                                                                  |
| **Beep**            | Plays a tone (frequency, duration, amplitude)                                                                                                                        |
| **Play sound**      | Plays a specific sound file from storage                                                                                                                             |
| **Vibrate**         | Vibrates the device (duration + pattern)                                                                                                                             |
| **Run task**        | Runs any Tasker task, with `par1`/`par2` plus arbitrary "pass variables" key–value pairs forwarded as Tasker variables, a return-variable name, and its own cooldown |
| **Set variable**    | Sets a Tasker variable to a fixed value                                                                                                                              |
| **Toggle variable** | Flips a Tasker variable between two values (A/B) each time it fires                                                                                                  |
| **Send command**    | Sends a raw command string (e.g. `wuzzapp=:=*`) — for advanced/internal use                                                                                          |
| **Dismiss**         | Closes a specific FAO/Notify card by ID — useful for clearing a persistent FAO once you've handled it                                                                |
| **Wait**            | Pauses (a random duration between min/max) before the next action in the chain runs                                                                                  |

## Configuring an alert card

Common fields across Flash / Notify / FAO:

| Field               | Description                                                                                                                                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Icon**            | Icon URI shown on the alert.                                                                                                                                                                                                                |
| **Text / Template** | The alert's message. Templates have a **before** and **after** variant plus a **threshold (ms)** — "before" is used while the event is fresh, "after" once the threshold has passed. Supports variables (see below) and basic HTML (`<b>`). |
| **Conditions**      | Gates that must pass for the card to fire — see [Conditions](#conditions) below.                                                                                                                                                            |
| **Cooldown**        | Minimum time before this card can fire again. **Duration** in ms, plus a **scope**: `card` (one shared timer for everyone) or `sender` (a separate timer per sender — and this timer can extend across other events too).                   |

Type-specific fields:

- **Flash** — timeout (ms), position (Top/Bottom/etc.), icon size, background color, text color, flash ID (used to target it with Dismiss, **dropped temporarily due to tasker bug**).
- **Notify** — group, category, priority, critical text (short text for compact/critical display).
- **FAO** — screen position (X/Y as %), show/dismiss animation, auto-dismiss timer, screen ID, icon tint.

### Interactions

Alert cards can define interactions for their available slots (Tap, Hold, Multi-tap, Swipe, or Button 1–3 for Notify). Each interaction:

- Has its own name/label
- Has its own **conditions**
- Runs a chain of one or more **cards** (Flash, Notify, Run Task, Say, Vibrate, Wait, Dismiss, etc.) in order, drag-reorderable

This is how you rebuild the old "tap the alert to open the chat" behavior: add a **Run Task** action under the Flash card's **Tap** interaction, pointing at **WUZ - Go To Chat**.

## Configuring an action card

Action cards (Say, Beep, Run Task, Set/Toggle Variable, Send Command, Wait, Dismiss) run automatically as soon as the event fires — there's no separate "trigger mode" toggle anymore, since automatic-vs-interactive is now just "is this card standalone, or is it nested inside another card's interaction."

**Run Task**:

| Field               | Description                                                                 |
| ------------------- | --------------------------------------------------------------------------- |
| **Task name**       | Autocompletes against any registered presets or enter manually.             |
| **par1 / par2**     | Standard Tasker parameters.                                                 |
| **Pass Variables**  | Extra key–value pairs forwarded into the task as Tasker variables (`%var`). |
| **Priority**        | Tasker task priority. 1-50 or -1 for tasker equivalent of `%priority-1`     |
| **Return variable** | Variable name the task's result is written back to.                         |
| **Cooldown**        | Same duration/scope model as alert cards.                                   |

## Conditions

Every card and every interaction has its own independent conditions block:

| Condition          | Behavior                                                                                                                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Chats / Groups** | Toggle whether this card applies to 1:1 chats and/or group chats. (Not shown for Status and Connection events, which aren't chat-scoped.)                                                 |
| **Sender**         | Restrict by who triggered the event: toggle **Others** and/or **Me**, plus an optional **allow/block** list of specific contacts. Mode defaults to `allow` (empty list = no restriction). |
| **Time**           | A time-of-day window, with an allow/block mode like the other list-based conditions (defaults to `allow` — only fires inside the window).                                                 |
| **Days**           | Pick specific days, with an allow/block mode (defaults to `block` — picked days are excluded).                                                                                            |
| **Apps**           | Pick specific apps by package, with an allow/block mode (defaults to `block` — card won't fire while a picked app is foregrounded). Requires App Manager project by WhirlWolf.            |

Every list-based condition (Time, Days, Apps, Sender) has its own **allow**/**block** switch, so you can flip a condition from "only these" to "everything except these" without rebuilding the list.

## Variables available in templates and tasks

| Variable        | Value                                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------- |
| `%sender.name`  | Display name of the contact who triggered the event                                                              |
| `%sender.phone` | Phone number of the contact who triggered the event                                                              |
| `%sender.lid`   | The contact's WhatsApp LID (linked ID)                                                                           |
| `%sender.id`    | Internal sender ID (used by default for Flash's `flashId`, and by Dismiss to target a specific card)             |
| `%sender.color` | A per-sender color, used by default for Flash/FAO backgrounds so different contacts are visually distinguishable |
| `%time`         | Timestamp of the event in hh.mm 12 hours format.                                                                 |

> Older versions of this screen used `%contact.name`, `%contact.phone`, and `%contact.lid`. These have been replaced by the `%sender.*` variables above — if you're rebuilding automations from an old export, update your variable references.

## Saving, resetting, and defaults

- **Save** validates fields (no negative cooldowns/thresholds, sensible min/max ranges) before writing your configuration.
- Each event section can be reset independently, or use **Reset All** to restore every event to its default single Flash card with no interactions.
- Defaults ship with **no Run Task cards and no interactions wired up** — if you want tapping a Flash to jump into a chat, send text message, etc., you need to add that Run Task under the relevant interaction yourself (see [Interactions](#interactions) above).
