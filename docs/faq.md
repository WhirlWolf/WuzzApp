# FAQ

**Does WuzzApp use the official WhatsApp API?**
No. It uses [wuzapi](https://github.com/asternic/wuzapi), a linked-device connection like WhatsApp Web. Not affiliated with WhatsApp. See [Safety & Privacy](safety-and-privacy.md).

**Does my phone need to stay online?**
Yes. wuzapi needs a connection and Tasker must run in the background. See [Background reliability](troubleshooting.md#background-reliability).

**Can I use more than one WhatsApp account?**
You can create several wuzapi users ([Configuration](configuration.md#the-wuzapi-user)), but the Tasker project handles one session at a time. Not officially supported.

**Termux or the server keeps stopping.**
See [Background reliability](troubleshooting.md#background-reliability).

**Automations are late or not firing.**
See [Automations not firing or firing late](troubleshooting.md#automations-not-firing-or-firing-late).

**Why am I always "online"?**
Expected while connected. Adjust under **WhatsApp → Settings → Privacy → Last Seen & Online**.

**Tapping an alert doesn't open the chat.**
Nothing is wired to taps by default. See [Interactions](event-preferences.md#interactions).

**How do I get structured logs?**
Run `./wuzapi -logtype json`.

**I'm stuck in a message/alert loop.**
See [Stuck in a loop](troubleshooting.md#stuck-in-a-loop-of-alerts-or-messages).

**Login says "Invalid credentials" or users are missing after an update.**
See [Users missing after update](troubleshooting.md#users-missing-or-invalid-credentials-after-update).

**Is my data sent anywhere?**
Not by WuzzApp. See [Safety & Privacy](safety-and-privacy.md).

**Can I expose the server to the internet?**
Not without protection in front of it (VPN, or a reverse proxy with auth). The dashboard and API aren't built for public exposure.
