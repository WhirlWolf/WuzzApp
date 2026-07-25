# FAQ

**Does WuzzApp use the official WhatsApp API?**
No. It's built on [wuzapi](https://github.com/asternic/wuzapi), which connects as a linked device using the same protocol as WhatsApp Web/Desktop. It is not affiliated with WhatsApp — see [Safety & Privacy](safety-and-privacy.md).

**Do I need to keep my phone connected to the internet all the time?**
Yes — wuzapi needs an active connection to receive WhatsApp events, and Tasker needs to run in the background to react to them. See [Troubleshooting → Background reliability](troubleshooting.md#background-reliability).

**Can I run this on more than one WhatsApp account?**
You can create multiple users in wuzapi's local database (see [Configuration](configuration.md#the-wuzapi-user)), but the Tasker project is built around a single active session. Running two fully in parallel isn't officially supported.

**Why is WuzzApp not working reliably in the background, or why does Termux/the server stop running?**
See [Troubleshooting → Background reliability](troubleshooting.md#background-reliability).

**Some automations aren't triggering, or are delayed. What should I do?**
See [Troubleshooting → Automations not firing or firing late](troubleshooting.md#automations-not-firing-or-firing-late).

**Why does my WhatsApp show me as "online" frequently?**
Expected behavior while connected. Adjust visibility under **WhatsApp → Settings → Privacy → Last Seen & Online**.

**Why does tapping a "composing" flash alert open a chat?**
It doesn't, by default — nothing is wired to alert taps until you set it up yourself. See [Event Preferences → Interactions](event-preferences.md#interactions).

**How do I enable colored/structured logs?**
Restart the server with `./wuzapi -logtype json`.

**What do I do if I get stuck in a message/alert loop?**
See [Troubleshooting → Stuck in a loop of alerts or messages](troubleshooting.md#stuck-in-a-loop-of-alerts-or-messages) for how to stop it and prevent it happening again.

**Is my data sent anywhere?**
No, not by WuzzApp itself. See [Safety & Privacy](safety-and-privacy.md).

**Can I expose the server to the internet so I can use it remotely?**
You can, but don't do it without authentication/protection in front of it (e.g. a VPN or reverse proxy with auth) — the dashboard and API are not designed to be publicly exposed as-is.
