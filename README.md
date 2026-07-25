# WuzzApp

Mod-level WhatsApp features. Tasker-level automation. React to anything, automate everything.

Uses [WuzAPI](https://github.com/asternic/wuzapi).

## Description

WuzzApp lets you control WhatsApp using Tasker (an Android automation app) — so you can build custom automations tied to WhatsApp activity, reacting to things like messages, status updates, or typing in real time and triggering actions on your phone automatically.

## Disclaimer

This project is **not affiliated with WhatsApp**. Use it responsibly and at your own risk — you are solely responsible for how you use it and any consequences that may follow, including account restrictions or bans.

## 📖 Documentation

- [Getting Started](docs/getting-started.md)
- [User Guide](docs/user-guide.md)
- [Event Preferences](docs/event-preferences.md)
- [Configuration](docs/configuration.md)
- [Updating](docs/updating.md)
- [Troubleshooting](docs/troubleshooting.md)
- [FAQ](docs/faq.md)
- [Safety & Privacy](docs/safety-and-privacy.md)

**Where to start:**

| If you're... | Read |
|---|---|
| Installing for the first time | [Getting Started](docs/getting-started.md) → [Configuration Reference](docs/configuration.md) |
| Setting up alerts/automations | [User Guide](docs/user-guide.md) → [Event Preferences](docs/event-preferences.md) |
| Upgrading from an older version | [Event Preferences Changelog](docs/event-preferences-changelog.md) → [Updating](docs/updating.md) |
| Something's not working | [Troubleshooting](docs/troubleshooting.md) → [FAQ](docs/faq.md) |
| Concerned about tokens/bans/exposure | [Safety & Privacy](docs/safety-and-privacy.md) |


## Features

**Real-Time Alerts** — status posts/deletes, read receipts, typing indicators, all configurable per event in [Event Preferences](docs/event-preferences.md)

**Contacts** — profile picture sync, number lookup

**Messaging** — send text, images, and locations directly from Tasker

**Automation** — modular cards (Flash/Notify/FAO/Run Task/Say/Vibrate/and more) with tap/hold/swipe interactions, plus conditional rules (sender, chat vs. group, time of day, days, foreground app)

**Updater support** via [Updater](https://github.com/WhirlWolf/Updater)

See the [User Guide](docs/user-guide.md) for details on every feature.

## Updating

The wuzapi library (Termux) and the Tasker project update independently. See **[Updating](docs/updating.md)** for the commands and steps — including the warning about re-importing the project erasing your existing Tasker-side setup.

## Privacy & Safety

WuzzApp itself does not collect or transmit any personal data — your data stays yours unless you set it up otherwise. See **[Safety & Privacy](docs/safety-and-privacy.md)** for guidance on avoiding bans, securing your tokens, and exposing the server safely.

## FAQ

See the full [FAQ](docs/faq.md) and [Troubleshooting](docs/troubleshooting.md) guides for background reliability, delayed automations, alert loops, and more.

## Contributing

Found a bug or have a feature request? [Open an issue](https://github.com/WhirlWolf/WuzzApp/issues) — all feedback is welcome.

---

## Support the Project

If you find WuzzApp useful, donate to keep it updated!

<a href="https://ko-fi.com/whirlwolf" target="_blank">
  <img src="https://cdn.ko-fi.com/cdn/kofi2.png?v=3" alt="Keep Wuzzapp Alive" width="150"/>
</a>
