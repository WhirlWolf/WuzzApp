# Safety & Privacy

**Disclaimer:** WuzzApp is **not affiliated with WhatsApp**. It uses [wuzapi](https://github.com/asternic/wuzapi), an unofficial linked-device connection. You are responsible for how you use it, including account restrictions or bans.

## Privacy

WuzzApp does NOT collect or send any personal data. The server, database and preferences all stay on your device, unless you set up your own external webhook or automation.

## Avoiding a WhatsApp ban

- **Avoid spam-like automations.** Triggers that instantly send messages on WhatsApp activity look bot-like.
- **Watch for loops.** **Send Text Message** on a **Message Delivered** trigger keeps firing itself. There's no anti-loop protection yet. If it happens, see [Troubleshooting](troubleshooting.md#stuck-in-a-loop-of-alerts-or-messages).
- **Prefer interactions** (Tap, Hold, etc.) for sending actions, so nothing sends unless you act. See [Event Preferences](event-preferences.md#configuring-an-action-card).

## Securing your setup

- **Never expose wuzapi to the internet** without authentication (VPN, or reverse proxy with auth).
- **Never share** your admin token, encryption key, HMAC key, webhook URL or `.env`. They give access to your WhatsApp session.
- **Regenerate anything compromised** right away: [Regenerating keys](configuration.md#regenerating-keys).
