<div align="center">

<img width="100%" alt="header" src="https://capsule-render.vercel.app/api?type=waving&height=210&text=Lumetra%20Bot&fontAlign=50&fontAlignY=36&fontSize=56&desc=Mining%20%7C%20Tasks%20%7C%20Referral%20%7C%20Turnstile%20Verified%20%7C%20Multi-Account&descAlign=50&descAlignY=58"/>

<img alt="typing" src="https://readme-typing-svg.demolab.com?font=Inter&size=18&duration=3000&pause=650&center=true&vCenter=true&width=900&lines=Auto+Turnstile+Verification+%7C+Unlock+Mining;Auto+Mining+%7C+Start+8h+Session+%26+Claim+Accrual;Auto+Tasks+%7C+Open+%26+Claim+%7C+Retry+on+Cooldown;Auto+Referral+Claim+%7C+When+Available;Leaderboard+Report+%7C+Live+Per+Cycle"/>

<p>
  <img alt="platform" src="https://img.shields.io/badge/Platform-LumeTRA%20Miniapp-111111"/>
  <img alt="multi-account" src="https://img.shields.io/badge/Multi--Account-Supported-111111"/>
  <img alt="proxy" src="https://img.shields.io/badge/Proxy-Supported-111111"/>
  <img alt="author" src="https://img.shields.io/badge/by-Yuurisandesu-111111"/>
</p>

<p>
  <b>Lumetra Bot</b> is a full automation bot for the LumeTRA Telegram Miniapp.<br/>
  It handles the complete cycle: signing in automatically from initData, passing Cloudflare Turnstile verification to unlock mining, completing all tasks with automatic cooldown retry, starting and claiming the mining session, collecting referral rewards, and reporting the leaderboard, all running automatically across multiple accounts with per-account proxy support and a live countdown between cycles.<br/>
  Built and distributed by <b>Yuurisandesu</b>.
</p>

</div>

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Bot](#running-the-bot)
- [Features](#features)
- [File Structure](#file-structure)
- [Disclaimer](#disclaimer)

---

## Requirements

- Ruby `3.0+` (only needed to run the downloader script)

---

## Install Ruby

### Linux

```bash
sudo apt update && sudo apt install -y ruby git
```

### macOS

```bash
brew install ruby
```

### Windows

Download and run the installer from https://rubyinstaller.org. Check **Add Ruby executables to your PATH** during installation. Verify with:

```bash
ruby --version
```

### Termux (Android)

```bash
pkg update && pkg install ruby git
```

---

## Installation

**Clone the repository:**

```bash
git clone https://github.com/Yuurisan-N1/Lumetra-Miniapp.git
cd Lumetra-Miniapp
```

**Download the binary for your platform:**

```bash
ruby bot.rb
```

The script shows a numbered menu:

```
1. Android ARM64
2. Android ARMv7
3. Linux aarch64
4. Linux armv7
5. Linux x86_64
6. macOS aarch64
7. Windows x86_64
```

Enter the number for your platform. The binary downloads with a live progress bar and is set to executable automatically on Linux, macOS, and Android.

Or download manually from the Releases page:
https://github.com/Yuurisan-N1/Lumetra-Miniapp/releases/latest

| File | Platform |
|---|---|
| `lumetra-bot-linux-x86_64` | Linux x86_64 |
| `lumetra-bot-linux-aarch64` | Linux ARM64 |
| `lumetra-bot-linux-armv7` | Linux ARMv7 |
| `lumetra-bot-macos-aarch64` | macOS Apple Silicon |
| `lumetra-bot-android-aarch64` | Android ARM64 (Termux) |
| `lumetra-bot-android-armv7` | Android ARMv7 (Termux) |
| `lumetra-bot-windows-x86_64.exe` | Windows x86_64 |

**Linux / macOS / Android after manual download:**

```bash
chmod +x lumetra-bot-linux-x86_64
```

Place the binary in the same folder as your `data.txt`, `proxy.txt`, and `config.json` before running.

---

## Configuration

### 1. Accounts (data.txt)

Fill `data.txt` with Telegram WebApp `initData` for each account, one per line:

```
user=%7B%22id%22...&hash=abc123
user=%7B%22id%22...&hash=def456
```

> `initData` can be obtained from the browser DevTools when opening LumeTRA on Telegram Web.

### 2. Proxy (proxy.txt)

Fill `proxy.txt` with proxies, one per line (optional, leave empty to run without proxy):

```
host:port
host:port:user:pass
http://user:pass@host:port
```

Proxies are assigned to accounts by index in round-robin order. Special characters in passwords such as `#`, `?`, `/`, and `;` are encoded automatically so they cannot break the proxy URL.

### 3. Bot Settings (config.json)

`sleep_seconds` controls how many seconds the bot waits between cycles. If `config.json` is missing, the bot falls back to a default of `3600` seconds.

---

## Running the Bot

**Linux / macOS / Android (Termux):**

```bash
./lumetra-bot-linux-x86_64
```

**Windows:**

```bash
.\lumetra-bot-windows-x86_64.exe
```

Press `Ctrl+C` at any time to stop the bot cleanly.

---

## Features

### Automatic Sign In
The bot parses the raw initData from `data.txt` and opens a server session for each account automatically. No manual steps are required.

### Turnstile Verification
Before mining can start, LumeTRA requires the account to pass Cloudflare Turnstile verification. The bot submits the challenge to a solving service, waits for the token, and completes the verification step automatically. Mining is only attempted after verification passes.

### Auto Tasks
The bot fetches all available tasks and works through each one in order. Each task is opened then claimed. Recurring tasks that are still on cooldown are detected automatically and retried after their cooldown expires. Progress is shown live in the terminal as an in-place counter while the task list is being processed.

### Mining Session
The bot starts the 8-hour mining cycle, claims any accumulated accrual from a previous session, and reports the remaining time of the current session. If the session is still running, the remaining duration is shown and mining is left to continue.

### Referral Claim
The bot checks whether a referral reward is available and claims it immediately when the server confirms one is ready.

### Leaderboard Report
At the end of each account cycle, the bot fetches the current leaderboard and logs the total player count and the top earner for that cycle.

### Multi Account
All accounts in `data.txt` are processed sequentially within every cycle. Each account runs its full cycle before moving to the next.

### Per Account Proxy
Proxies are loaded from `proxy.txt` and assigned to accounts by position in round-robin order. Special characters in proxy credentials are encoded automatically. Running without proxies is fully supported.

### Live Progress and Clean Stop
Long task walks show an in-place counter that updates without flooding the terminal. Every wait period shows a live `HH:MM:SS` countdown that erases itself when it reaches zero. Pressing `Ctrl+C` at any point stops the bot cleanly and exits.

---

## File Structure

```text
Lumetra-Miniapp/
├── lumetra-bot-linux-x86_64        # Linux x86_64 binary
├── lumetra-bot-linux-aarch64       # Linux ARM64 binary
├── lumetra-bot-linux-armv7         # Linux ARMv7 binary
├── lumetra-bot-macos-aarch64       # macOS Apple Silicon binary
├── lumetra-bot-android-aarch64     # Android ARM64 binary (Termux)
├── lumetra-bot-android-armv7       # Android ARMv7 binary (Termux)
├── lumetra-bot-windows-x86_64.exe  # Windows binary
├── bot.rb                          # Interactive downloader script (Ruby)
├── config.json                     # Sleep duration between cycles
├── data.txt                        # Account initData, one per line
├── proxy.txt                       # Proxy list, one per line (optional)
├── LICENSE                         # License file
└── utils/
    └── banner.rb                   # Banner using yuurisan module
```

---

## Disclaimer

This tool is built for educational and technical exploration purposes. Use it wisely and at your own responsibility.

---

<div align="center">
<img width="100%" alt="footer" src="https://capsule-render.vercel.app/api?type=waving&height=120&section=footer"/>
</div>