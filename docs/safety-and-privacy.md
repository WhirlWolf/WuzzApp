# Safety & Privacy

## Disclaimer

WuzzApp is **not affiliated with WhatsApp**. It connects using [wuzapi](https://github.com/asternic/wuzapi), an unofficial linked-device connection. Use it responsibly — you are solely responsible for how you use it and any consequences, including account restrictions or bans.

## Privacy

WuzzApp itself does not collect or transmit any personal data. Everything — the server, the database, your event preferences — runs and stays on your device.

**Your data stays yours unless you set up otherwise** (for example, if you configure your own external webhook or automation that sends data elsewhere).

## Avoiding a WhatsApp ban

- **Avoid aggressive or spam-like automations.** Automatic triggers that immediately send messages in response to WhatsApp activity can easily cross into behavior WhatsApp flags as bot-like.
- **Watch out for loops.** For example, wiring **Send Text Message** to a **Message Delivered** trigger will cause it to keep firing itself, sending repeatedly. There's no built-in anti-loop protection yet — be deliberate about which triggers you set to automatic. See [Troubleshooting](troubleshooting.md#stuck-in-a-loop-of-alerts-or-messages) if this happens.
- Prefer attaching message-sending actions to an **interaction** (Tap, Hold, etc. — requires you to act) rather than running them automatically on every event, unless you're confident in the logic — see [Event Preferences](event-preferences.md#configuring-an-action-card).

## Securing your setup

- **Never expose your wuzapi server to the public internet** without authentication in front of it (e.g. a VPN, or a reverse proxy with its own auth layer). The dashboard and API assume a trusted local network.
- **Never share** your admin token, global encryption key, global HMAC key, webhook URL, or `.env` file. Anyone with these can access your WhatsApp session.
- **If anything is compromised, regenerate it immediately** — see [Configuration → Regenerating keys](configuration.md#regenerating-keys).
