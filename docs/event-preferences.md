# Event Preferences

Open **WUZ - Event Preferences** in Tasker to set how WuzzApp reacts to each WhatsApp event.

## Events

12 events, each its own collapsible section:

- **Connection:** Connected, Disconnected, Logged Out
- **Status:** Status Posted, Status Deleted
- **Messages:** Delivered, Read, Received, Sent, Deleted
- **Presence:** Chat Presence Composing, Chat Presence Paused

Each starts with one default **Flash** card and no interactions.

## Cards

A card is one unit of behavior. Add as many as you like per event. Drag to reorder, long-press the drag handle to duplicate.

### Alert cards

Show something to you. They can have **interactions**.

| Card | Shows | Interactions |
|---|---|---|
| **Flash** | Temporary overlay, colored per sender | Tap |
| **Notify** | Android notification, up to 3 buttons | Button 1-3 |
| **FAO** (Floating Action Overlay) | Persistent bubble/badge until dismissed | Tap, Hold, Multi-tap, Swipe |

### Action cards

Run automatically when the event fires. Also used inside an alert's interactions (e.g. on tap, Run Task).

| Card | Does |
|---|---|
| **Say** | Speaks text (voice, pitch, speed) |
| **Beep** | Plays a tone (frequency, duration, amplitude) |
| **Play sound** | Plays a sound file |
| **Vibrate** | Vibrates (duration, pattern) |
| **Run task** | Runs a Tasker task, with `par1`/`par2`, extra variables, a return variable and a cooldown |
| **Set variable** | Sets a Tasker variable |
| **Toggle variable** | Flips a variable between two values each time |
| **Send command** | Sends a raw command string (e.g. `wuzzapp=:=*`). Advanced. |
| **Dismiss** | Closes a FAO/Notify card by ID |
| **Wait** | Pauses a random time (min-max) before the next action |

## Configuring an alert card

Fields shared by Flash, Notify and FAO:

| Field | Description |
|---|---|
| **Icon** | Icon URI shown on the alert. |
| **Text / Template** | The message. Has a **before** and **after** variant and a **threshold (ms)**: "before" is used while the event is fresh, "after" once the threshold passes. Supports [variables](#variables) and basic HTML (`<b>`). |
| **Conditions** | Must pass for the card to fire ([Conditions](#conditions)). |
| **Cooldown** | Minimum time before the card can fire again. **Duration** in ms and **scope**: `card` (one shared timer) or `sender` (one timer per sender, can span other events). |

Type-specific fields:

- **Flash:** timeout (ms), position, icon size, background and text color, flash ID (for Dismiss; **temporarily dropped due to a Tasker bug**).
- **Notify:** group, category, priority, critical text (short text for compact display).
- **FAO:** position (X/Y %), show/dismiss animation, auto-dismiss timer, screen ID, icon tint.

### Interactions

Alert cards can define interactions for their slots (Tap, Hold, Multi-tap, Swipe, or Button 1-3 for Notify). Each interaction has a name, its own **conditions**, and a drag-reorderable chain of cards that run in order.

**Example, tap to open the chat:** under the Flash card's **Tap** interaction, add a **Run Task** pointing at **WUZ - Go To Chat**. Defaults ship with no interactions, so add this yourself.

## Configuring an action card

Action cards run as soon as the event fires. A card is automatic when standalone and interactive when nested inside another card's interaction.

**Run Task fields:**

| Field | Description |
|---|---|
| **Task name** | Autocompletes from presets, or type it. |
| **par1 / par2** | Standard Tasker parameters. |
| **Pass Variables** | Extra key-value pairs sent to the task as `%var`. |
| **Priority** | Tasker priority, 1-50, or -1 for `%priority-1`. |
| **Return variable** | Variable that receives the task's result. |
| **Cooldown** | Same duration/scope as alert cards. |

## Conditions

Every card and interaction has its own conditions.

| Condition | Behavior |
|---|---|
| **Chats / Groups** | Apply to 1:1 chats and/or groups. Hidden for Status and Connection events. |
| **Sender** | Toggle **Others** and/or **Me**, plus an optional allow/block list of contacts. Defaults to `allow` (empty list = no restriction). |
| **Time** | Time-of-day window. Defaults to `allow` (fires only inside the window). |
| **Days** | Specific days. Defaults to `block` (picked days excluded). |
| **Apps** | Specific apps by package. Defaults to `block` (won't fire while a picked app is in the foreground). Needs the App Manager project by WhirlWolf. |

Time, Days, Apps and Sender each have an **allow/block** switch, so you can flip "only these" to "all except these" without rebuilding the list.

## Variables

Usable in templates and tasks.

| Variable | Value |
|---|---|
| `%sender.name` | Contact's display name |
| `%sender.phone` | Contact's phone number |
| `%sender.lid` | Contact's WhatsApp LID |
| `%sender.id` | Internal sender ID (default Flash `flashId`; Dismiss uses it) |
| `%sender.color` | Per-sender color for Flash/FAO backgrounds |
| `%time` | Event time, hh.mm 12-hour |

## Saving and resetting

- **Save** validates fields (no negative cooldowns or thresholds, sensible min/max) first.
- Reset each event on its own, or use **Reset All** to restore one default Flash card per event with no interactions.
- Defaults have **no Run Task cards or interactions**. To make a tap open a chat or send text, add the Run Task under that interaction ([Interactions](#interactions)).
