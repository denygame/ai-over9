# Telegram guide

[🇻🇳 Vietnamese](telegram.vi.md) · [← Back to the README](../README.md)

Telegram is AI Over9's remote control: notifications from your agents, status and actions from anywhere, and — if
you turn it on — a chat with every agent session on your computer.

- [Create your bot and pair](#create-your-bot-and-pair)
- [People and roles](#people-and-roles)
- [Notifications](#notifications)
- [Commands](#commands)
- [Chatting with your agents](#chatting-with-your-agents)
- [Continue after a usage limit](#continue-after-a-usage-limit)
- [Security](#security)
- [Troubleshooting](#troubleshooting)

<img src="../images/telegram-chat.png" alt="Telegram: agent answers and replies going back into each session (sample data)" width="400">

---

## Create your bot and pair

You use **your own bot** — one per computer. Nothing goes through a server of ours.

1. In Telegram, open [@BotFather](https://t.me/BotFather) and send `/newbot`. Choose a name and a username ending in
   `bot`. BotFather replies with a **token** like `123456789:AA…`.
2. In AI Over9: **Integrations › Telegram**. Paste the token and click **Save to Keychain**. The app checks it with
   Telegram and shows *Bot verified: @yourbot*.
   - The token is stored **only in the macOS Keychain** (Windows: Credential Manager; Linux: the system keyring).
   - Never paste it into a chat — not with the bot, not with an AI.
3. Click **Create pairing code (you become Owner)**. Open a private chat with your bot and send exactly what the app
   shows, for example `/pair 7AB6NS2B`. The code works once and expires after a few minutes.
4. The bot answers *Paired … as Owner*. Send `/help` to see what you can do.

---

## People and roles

Add family or teammates with **Create code for another person**, or approve someone who sends a pairing request.

| Role | Can |
|---|---|
| **Viewer** | read status, sessions, usage, why the computer is awake |
| **Operator** | everyday actions: keep awake, Finish then sleep, cancel, sleep (with checks), fans to Automatic, allowed automations |
| **Owner** | everything: users, power, fan speeds, shutdown (if enabled) — and **chatting with agents** |

Remove someone in the app or with `/users` and `/revoke` (Owner).

---

## Notifications

What arrives, and when (Settings › Notifications and Integrations › Telegram):

- an agent **started**, **finished a turn** (with the usage left), **failed** or was **interrupted**;
- an agent **needs your approval**;
- a usage **limit** is almost used, reached, or available again;
- *Finish, then sleep* countdown, sleep, the app quitting, the computer shutting down, the lid closed while agents work;
- fan and thermal safety (macOS).

**Only when I am away:** turn messages wait until you have left the computer for 2 minutes, so your phone stays quiet
while you work. Approvals and limits always come through.

Commands sent while the computer slept are not replayed one by one: you get a short summary instead. Project names can
be hidden (*Include project folder names in messages*).

---

## Commands

| Command | Role | What it does |
|---|---|---|
| `/status`, `/menu` | Viewer | Overview with buttons |
| `/why` | Viewer | Why the computer is awake, what still blocks sleep |
| `/agents`, `/sessions` | Viewer | Agents and sessions |
| `/usage`, `/reset` | Viewer | Plan limits, reset times, tokens |
| `/awake` · `/awake 30` · `/awake off` | Viewer · Operator | Status · keep awake 30 more minutes · stop |
| `/sleep`, `/cancel` | Operator | Sleep after checks and a countdown · cancel |
| `/fans`, `/fans auto`, `/fans cool` | Viewer · Operator | Fans (macOS) |
| `/fans set <1/2/all> <rpm>`, `/fans profile <name>` | Owner | Constant speed, profiles (if allowed) |
| `/mission`, `/missions`, `/report` | Viewer | Tonight's work |
| `/battery`, `/power`, `/profile <name>` | Viewer · Operator | Battery, power modes |
| `/schedule`, `/automation`, `/run <name>` | Viewer · Operator | Schedules, allowed automations |
| `/devices`, `/device <name> on/off` | Viewer · Operator | Smart devices |
| `/unlock <code>`, `/lock` | Owner | Open / close chatting with agents |
| `/users`, `/revoke` | Owner | People |
| `/lang en` · `/lang vi`, `/help` | Viewer | Language, help |

Risky actions ask for a confirmation button; permissions are checked again when you press it. The bot never accepts a
shell command.

---

## Chatting with your agents

### Turn it on

**Settings › Dangerous actions** (off by default):

1. **Send the agents' final answers to the Owner on Telegram** — confirm.
2. **Let the Owner write into agent sessions from Telegram** — scan the QR code with an authenticator app (Google
   Authenticator, 1Password, Authy…), enter the 6-digit code, **Turn on**.
3. **Unlock lasts** 1 hour by default (up to 24). **Lock now** ends it; **New authenticator…** replaces the key.

<img src="../images/shot-dangerous.jpg" alt="Settings › Dangerous actions" width="760">

### Answers

When an agent finishes a turn, its final answer is sent to the Owner:

```
💬 Claude Code · web-shop
🧵 Add checkout tests

Done. 12 tests added for the checkout flow …

↩️ Reply to this message to continue the session.
```

- the agent, the project and the **session title** (🧵) — so you always know which conversation it is;
- formatting kept: bold, italics, lists, links, code, code blocks, quotes; **tables** become aligned columns;
- very long answers are shortened, with a note; the full text stays in the session.

### Unlock

Writing needs an unlock with a code from your authenticator app: `/unlock 123456`. It lasts the time you chose, also
across restarts of the app. `/lock` closes it.

### Where your message goes

- **Reply to an answer** → that answer's session. The reliable way with several sessions.
- **A plain message** → the session of the latest answer you received.
- A reply to an answer the bot can no longer place is refused with a note — never sent to another session.

### What happens then

| Session | Your message | Bot says |
|---|---|---|
| Claude Code, open (terminal, VS Code, desktop) | delivered at once, even mid-turn; the agent starts its answer with `📱 Telegram: …` so the message also shows in the session window | 📨 Sent … (open now) |
| Codex, open and idle in VS Code / the ChatGPT app | that window starts the turn at once; shows there like a typed message | 📨 Sent … (open now) |
| Codex, busy | handed over at the end of the current turn | ⏳ … when its current turn ends |
| Closed session | continued in the background (`claude --resume` / `codex exec resume`); answer on Telegram; the session shows the turn when reopened | 🔄 … continues in the background |

If something cannot run (for example the agent's command-line tool is missing), the bot says why — a message is never
dropped silently.

---

## Continue after a usage limit

Optional rule in **Settings › Dangerous actions**: a session stopped by a usage limit gets your text (default
`continue`) once the limit resets — at most 3 times per session: open sessions get it like any message, closed ones
continue in the background. Until the reset the computer stays awake, so *Finish, then sleep* waits for the session to
continue and finish instead of sleeping through the reset.

---

## Security

- **Owner only.** Viewers and Operators never receive answers and cannot write to agents.
- **Second factor.** Writing needs a code from your authenticator app, not from Telegram: a stolen Telegram account
  alone cannot drive your agents. Wrong codes are rate-limited (5 per 15 minutes). The code is never stored in the
  message history; the authenticator key lives only in the Keychain.
- **Nothing bypassed.** The agents' own permission rules apply to every turn. The bot never runs a command itself;
  background turns start the agent with fixed arguments and pass your text on its standard input.
- **Only your chat.** Answers go to the Owner's private chat with your own bot. Nothing is stored by AI Over9 beyond
  the message history on your computer.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| The bot does not answer | Integrations › Telegram: the status must be *connected*. A second app polling the same bot token causes *conflict*: one bot per computer. |
| `/unlock` again and again | The unlock period ended, `/lock` was sent, or the authenticator was changed. |
| "Wrong code" | Use the current code; check that the computer's clock is set automatically. |
| No 💬 answers | *Send the agents' final answers* is off, or the agent is not connected ([connect agents](connect-agents.md)). |
| Codex: "⏳ when its current turn ends" | The session is busy — wait, or check that it is not open in a Codex window that does not respond. |
| Old commands after waking up | Commands older than 2 minutes are summarised, not run. |
