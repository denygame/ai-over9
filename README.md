<div align="center">

# 🌙 AI Over9

**Your AI works. Your machine rests.**

A native macOS menu bar app for people who let AI coding agents work for them — Claude Code, Codex, Cursor, Gemini and more.
AI Over9 keeps your Mac awake while the agents work, puts it to sleep when they are done, keeps it cool, and tells you everything on Telegram.

[**⬇️ Download the latest release**](https://github.com/denygame/ai-over9/releases/latest) · macOS 14 Sonoma or later · Apple silicon

</div>

---

## Contents

- [Why AI Over9](#why-ai-over9)
- [Quick actions](#quick-actions)
- [Features](#features)
  - [AI agent monitoring](#ai-agent-monitoring)
  - [Finish, then sleep](#finish-then-sleep)
  - [Keep awake](#keep-awake)
  - [Fans and cooling](#fans-and-cooling)
  - [Usage limits](#usage-limits)
  - [Telegram remote](#telegram-remote)
  - [Notifications](#notifications)
  - [Lock screen status](#lock-screen-status)
  - [Claude Remote Control](#claude-remote-control)
  - [Automations, schedules and power modes](#automations-schedules-and-power-modes)
  - [Smart devices](#smart-devices)
  - [System and storage](#system-and-storage)
- [Installation](#installation)
- [First-time setup](#first-time-setup)
- [Updates](#updates)
- [Privacy and security](#privacy-and-security)
- [Permissions](#permissions)
- [FAQ and troubleshooting](#faq-and-troubleshooting)
- [Uninstall](#uninstall)
- [License](#license)
- [Support](#support)

---

## Why AI Over9

AI coding agents can work for hours. Your Mac should not fall asleep halfway through a task, and it should not stay awake all night once the work is done.

AI Over9 sits in the menu bar and answers the questions you would otherwise keep checking:

- **Is the agent still working?** Every Claude Code and Codex session is tracked from its own hooks: started, working, waiting for your approval, finished, interrupted.
- **Will the Mac stay awake?** It is kept awake while there is work, and released when there is none. The display can still turn off.
- **Will it sleep when the work is done?** Turn on *Finish, then sleep* and it does, after a short countdown you can cancel.
- **Is it getting too hot?** Read the fans, cool the Mac with one click, or let it cool itself when the CPU gets hot.
- **How much of my plan is left?** See your 5-hour and weekly limits, with the real numbers from your account.
- **What happened while I was away?** Telegram tells you, and lets you check or act from your phone.

Everything runs locally on your Mac. There is no account, no analytics and no telemetry.

---

## Quick actions

The three things you open the app for, one click each — on the Overview and in the menu bar:

| | Action | What it does |
|---|---|---|
| 🌙 | **Finish, then sleep** | When every agent has finished and you are not using the Mac, AI Over9 shows a 2-minute countdown (cancel anytime) and puts the Mac to sleep. The tile tells you exactly what it is waiting for. |
| ❄️ | **Cool the Mac** | Runs the fans faster until the CPU is cool again, then returns them to Automatic. Optional: cool automatically when the CPU reaches 85 °C. |
| ☕ | **Keep awake** | For one hour, or until you stop it. |

---

## Features

### AI agent monitoring

- **Claude Code** (terminal and VS Code) and **Codex** are followed through their official hooks. AI Over9 installs its own small hook entries next to yours. Your configuration is merged and backed up, never replaced, and removing the hooks deletes only AI Over9's entries.
- **Other agents are detected automatically** from the running process, without any setup: Cursor Agent, Gemini CLI, Qwen Code, Kimi CLI, Aider, OpenCode, Goose, Amp, Crush and Factory Droid. Monitoring is more limited for these (start, working, exit), and is labelled as such.
- **Session states:** working, waiting for approval, idle, finished, interrupted, and unknown when a session goes silent. Short background sessions that never do any work are ignored.
- **Missions:** a night's work is grouped automatically when you turn on *Finish, then sleep*, and a report is shown when it is done.
- **"Why is my Mac awake?"** explains in plain words: which agents keep it awake, which other apps hold it awake, and what still blocks sleep.

### Finish, then sleep

- Sleeps only when it is safe:
  - every agent has finished, with nothing waiting for your approval;
  - you have not used the Mac for a few minutes;
  - no build tool is running.
- A cancellable countdown is shown in the menu bar and on Telegram before the Mac sleeps.
- If new work starts during the countdown, it is cancelled automatically.
- Shutdown is a separate, opt-in feature with a two-step confirmation.

### Keep awake

- AI Over9 uses its own macOS power assertion; no other app is needed.
- **Automatic mode:** the Mac stays awake while agents work, while an automation runs, during scheduled windows, and when you ask.
- **Manual mode:** one hour, a custom time, or until you stop it — from the app, the menu bar or Telegram.
- **Battery aware:** you are warned on low battery, and keep-awake can be released below a level you choose so macOS can protect the battery.

### Fans and cooling

- **Read** the speed of every fan, its range and its mode directly from the Mac. No helper is needed for reading.
- **Control** speeds through a small privileged helper that you install once with your administrator password:
  - **Constant speed** per fan, within the hardware limits of your Mac.
  - **Cool now:** a temporary cooling run that ends when the CPU is cool again (or after a time limit), then returns to Automatic.
  - **Auto-cool:** starts a cooling run by itself when the CPU reaches 85 °C.
  - **Saved fan profiles.**
- **Safety first:**
  - the helper only accepts fixed fan commands and checks every value against your Mac's limits;
  - the fans return to Automatic when you quit the app, when the temperature gets high, or if AI Over9 stops responding (watchdog);
  - AI Over9 never fights another fan app — while one is in control, AI Over9 only reads.
- Developed and tested on a MacBook Pro with M3 Max.

### Usage limits

- **Token usage** from your local Claude Code and Codex session files: today, the last 7 and 30 days, by agent, model and project.
- **Plan limits with real numbers** (optional, *Exact numbers*): the 5-hour and weekly percentages and reset times, read from your Claude and ChatGPT accounts using the sign-in those tools already stored on your Mac.
  - Tokens are used in memory for that request only and never stored.
  - These services are not officially documented and can change.
  - Percentages are only shown when they come from the provider — AI Over9 never guesses them.
- **Alerts** when a limit is almost used up, is reached, or is available again — as a message from the menu bar icon and on Telegram.

### Telegram remote

Pair your own Telegram bot (one bot per Mac). Roles control who can do what: **viewer** (read only), **operator** (everyday actions), **owner** (power and fan speeds).

| Command | What it does |
|---|---|
| `/status`, `/menu` | Mac overview and quick buttons |
| `/why` | Why the Mac is awake, and what still blocks sleep |
| `/agents`, `/sessions` | Agents and tracked sessions |
| `/usage`, `/reset` | Token usage, plan limits and reset times |
| `/awake`, `/awake 30`, `/awake off` | Keep-awake status, keep awake N more minutes, stop |
| `/sleep` | Request sleep (safety checks and countdown) |
| `/fans`, `/fans auto`, `/fans cool`, `/fans set <1/2/all> <rpm>` | Fan status, back to Automatic, cool now, constant speed (owner, if allowed) |
| `/fans profiles`, `/fans profile <name>` | Saved fan profiles |
| `/mission`, `/missions`, `/report` | Current mission, recent missions, last report |
| `/battery`, `/charging` | Battery, charger and charging state |
| `/power`, `/profiles`, `/profile <name>` | Power modes |
| `/schedule`, `/automation`, `/scenarios`, `/history` | Schedules and automations |
| `/run <scenario>`, `/cancel` | Run an allowed automation, cancel a running one |
| `/devices`, `/device <name> on/off` | Smart devices |
| `/health`, `/backup` | Health check, backup status |
| `/shutdown` | Graceful shutdown (two steps, only if enabled) |
| `/lang en` · `/lang vi` | Message language |

- **Every risky action needs a confirmation button**, and permissions are checked again when you press it.
- **Shell commands are never accepted.**
- **Messages:** agent started working, finished a turn (with the usage left), error or interrupted; approvals needed; usage limits; fan and thermal safety; the app quitting; the Mac shutting down; and a warning when the lid is closed while agents still keep the Mac awake.

### Notifications

- **Usage alerts** drop down from the menu bar icon as a small chat-style message. Other notifications use Notification Center and Telegram.
- **Quiet hours**, a minimum severity, a per-minute limit and per-kind rules.
- **"Only when I am away":** agent turn messages are only sent when you have not used the Mac for 2 minutes, so your phone stays quiet while you work. Approvals and limits are always sent.

### Lock screen status

- While the Mac is locked and an agent works, a card below the clock shows the agents, how long they have been working, and any approval they need.
- When everything is done, it shows the sleep countdown.
- It is display only and can be turned off.

### Claude Remote Control

- One switch turns on Claude Code's Remote Control for every new session, in the terminal and in VS Code, so you can continue them from claude.ai or the Claude app.
- Your previous Claude setting is backed up and restored when you turn it off.
- If your organization disables Remote Control, AI Over9 shows it and changes nothing.
- Requires a Claude Pro, Max, Team or Enterprise plan.

### Automations, schedules and power modes

*For power users — shown with Advanced mode.*

- **Automations:** "when something happens, if conditions are met, then do this". Built-in examples:
  - smart overnight completion;
  - usage limit reached;
  - approval needed;
  - thermal safety;
  - agent turn notifications.
- **Schedules:** run an automation, keep the Mac awake during a time window, or send a report at a set time. Missed runs are handled safely.
- **Power modes:** Overnight Coding, Battery Saver, Performance and Always Awake — one switch for a set of keep-awake, battery and notification preferences.
- **Safety:**
  - every action that can sleep or shut down the Mac, control fans or devices, quit apps or run scripts needs its own permission in Settings › Safety;
  - a dry-run mode exists for testing.

### Smart devices

- **Home Assistant:** pick the devices AI Over9 may see. Control is a separate permission for each device, and updates arrive in real time.
- **Custom HTTP devices** on your local network, with strict limits: same host only, no redirects, size limits.
- **MacBook charging is monitored only.** AI Over9 never changes charging and never cuts power.

### System and storage

- **Live system view:** CPU, memory, temperature, disk space and network, in the app and in the menu bar.
- **Storage:** how much space agent sessions, caches and build folders (`node_modules`, `.build`, `dist`, …) use. AI Over9 never deletes anything by itself.
- **Backup and restore** of your settings, automations and profiles. Secrets are never included.

---

## Installation

1. Download `AIOver9-<version>-macos-arm64.dmg` from the [latest release](https://github.com/denygame/ai-over9/releases/latest).
2. Open the DMG and drag **AI Over9** to **Applications**.
3. **First launch only:** the app is signed by its author but not notarized by Apple, so macOS asks once.
   - Right-click **AI Over9** in Applications › **Open** › **Open**, or
   - open **System Settings › Privacy & Security** and click **Open Anyway**.
4. A short setup assistant guides you through the first steps.

Each release includes a `.sha256` file to verify the download:

```bash
shasum -a 256 -c AIOver9-<version>-macos-arm64.dmg.sha256
```

---

## First-time setup

| Step | Where | Notes |
|---|---|---|
| **Agent hooks** | Integrations › Claude Code / Codex › Install | Lets AI Over9 see when your agents work. Your files are merged and backed up. |
| **Telegram** (optional) | Integrations › Telegram | Create a bot with [@BotFather](https://t.me/BotFather), paste its token **in the app** (stored in the macOS Keychain only), then pair your account from Telegram. |
| **Fan helper** (optional) | Fans & Power › Install fan helper… | Needed only to change fan speeds. macOS asks for your administrator password once. |
| **Exact numbers** (optional) | Usage › Exact numbers | Real plan limits from your Claude and ChatGPT accounts. |
| **Finish, then sleep** | Overview | One switch. |

---

## Updates

- AI Over9 updates itself:
  - it checks once a day, or whenever you choose **Settings › Updates › Check for updates**;
  - when a new version is out, a message drops down from the menu bar icon — click it to see what's new and install;
  - the app restarts by itself.
- Your settings, Telegram pairing, Keychain items and the fan helper are kept.
- **Security:** every update is verified before it is installed — it must carry the author's signature and be signed with the same certificate as the installed app.

---

## Privacy and security

- **Local first.** Sessions, missions, automations, usage numbers and history stay in `~/Library/Application Support/AIOver9` on your Mac.
- **No account, no analytics, no telemetry.**
- **Network access happens only for features you turn on:**
  - Telegram (your own bot);
  - Home Assistant / HTTP devices (the addresses you configure);
  - *Exact numbers* (your Claude / ChatGPT account usage);
  - update checks (GitHub).
- **Secrets** — bot token, Home Assistant token, device secrets — live only in the macOS Keychain. Never in files, logs or backups.
- **Agent content:**
  - only token counts are read from session files — your conversations are not stored;
  - project names in Telegram messages are optional.
- **Telegram:**
  - only paired users, with roles;
  - risky actions need a confirmation;
  - no shell commands, ever.
- **Fan helper:** a small root helper that accepts only fixed fan commands and verifies that it talks to the genuine AI Over9 app.

---

## Permissions

| macOS may ask for | Why | When |
|---|---|---|
| Notifications | Alerts on this Mac | First launch |
| Administrator password | Installing the fan helper | Only if you install it |
| Automation (System Events) | Graceful shutdown or screen lock | Only if you enable those automations |
| Accessibility | Sending the screen-lock shortcut | Only if you use the lock-screen automation |
| Documents / Desktop / Downloads | Measuring build folders in your projects | Only when you scan storage there |

---

## FAQ and troubleshooting

**The Mac did not sleep after the agents finished.**
It waits until you have left the Mac alone for a few minutes and nothing is still running. The 🌙 tile and `/why` on Telegram show what it is waiting for.

**No Telegram message when an agent finished a turn.**
Check Settings › Notifications › "Only when I am away": while you are using the Mac, turn messages are not sent.

**The fan helper is not responding.**
Use Fans & Power › Reinstall fan helper…. Updates ask for this only when the helper itself has changed.

**Fan control stays read-only.**
Another fan app is probably in control. Set it to Automatic and quit it, or confirm in AI Over9 that it may take over.

**Usage percentages are missing.**
Turn on *Exact numbers* on the Usage page. Without it, AI Over9 shows token counts only — it never estimates plan percentages.

**macOS says the app cannot be opened.**
See [Installation](#installation), step 3. This is needed once.

---

## Uninstall

1. **Optional, from the app:**
   - remove the agent hooks (Integrations › Claude Code / Codex);
   - remove the fan helper (Fans & Power › Remove fan helper; asks for your administrator password);
   - turn off Remote Control if you turned it on.
2. Quit AI Over9 from the menu bar, then delete **AI Over9** from Applications.
3. **Optional:** delete its data:
   ```bash
   rm -rf ~/Library/Application\ Support/AIOver9
   ```
   Keychain items are listed under `dev.huynt.aiover9` in Keychain Access.

---

## License

- Copyright © 2026 HuyNT. All rights reserved.
- AI Over9 is provided "as is", without warranty of any kind. You use automations that can sleep or shut down your Mac, or control fans or devices, at your own risk.
- **Third-party software:**
  - [Sparkle](https://sparkle-project.org) (MIT) for updates;
  - native fan control was written for AI Over9, informed by the MIT-licensed projects [AutoFansMac](https://github.com/garyhooi/AutoFansMac), [MacFanControl](https://github.com/raminsharifi/MacFanControl) and [macsfan](https://github.com/matejrondzik/macsfan).

---

## Support

AI Over9 is free, with no ads and no tracking. If it makes your nights a little easier, you can buy the author a coffee from **About** in the app — entirely optional; the app stays the same either way.

Questions, ideas or bugs: open an [issue](https://github.com/denygame/ai-over9/issues).
