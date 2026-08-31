<div align="center">

<img src="./assets/fermi-icon.png" width="120" alt="Fermi icon" />

# Fermi

**A lightweight macOS app for undergraduate physics, powered by Google Gemini.**

[![Latest Release](https://img.shields.io/github/v/release/SourodeepSarkar/fermi?label=latest%20release)](https://github.com/SourodeepSarkar/fermi/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/SourodeepSarkar/fermi/total)](https://github.com/SourodeepSarkar/fermi/releases)
[![macOS](https://img.shields.io/badge/macOS-13%2B-black?logo=apple)](https://github.com/SourodeepSarkar/fermi/releases/latest)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE.md)

[Download](#download) · [Setup](#first-launch-setup) · [Usage](#using-fermi) · [FAQ](#faq) · [Uninstall](#uninstalling)

</div>

---

## What is Fermi?

Fermi is a small macOS menu-bar-less **agent app** that sets up and launches a command-line study assistant for undergraduate physics. It walks you through a one-time setup — checking your Mac, downloading the engine, connecting a free Google Gemini API key, and (optionally) adding a `fermi` command to your terminal — then drops you straight into a fast, distraction-free chat interface in Terminal.

This repository hosts **pre-built releases of the Fermi.app** you can download and run directly — no Xcode, no building from source required. The application source lives in a separate repository; see [How it works](#how-it-works) below.

**Highlights**

- 🧠 **Adaptive answers.** Fermi sizes its response to the question — a quick concept check gets a few sentences, a genuinely hard derivation gets the full step-by-step treatment. It doesn't pad every reply with boilerplate code or unrelated sections.
- 🛠️ **Real tool use, not homework for you.** When a number needs computing or a fact needs verifying, Fermi runs Google's code-execution and web-search tools itself and reports the result, instead of just handing you a script to run.
- 📎 Attach PDFs, images, problem sets, or entire code directories for context-aware help.
- 💬 Multiple persistent, named study sessions — switch, create, or export them from inside the app.
- ⚙️ Pick your model (`flash` for speed, `pro` for harder problems) and temperature on the fly.
- ⚡️ A guided, macOS-native setup — no manual `pip install`, no editing config files by hand.
- 🔒 Your API key is stored locally in a `.env` file on your own Mac. It is never sent anywhere except Google's API.

---

## Download

1. Go to the [**Releases**](https://github.com/SourodeepSarkar/fermi/releases/latest) page.
2. Under **Assets**, download the latest `Fermi.app.zip` (or `Fermi.dmg`, if provided).
3. Unzip it (double-click) and drag **Fermi.app** into your `/Applications` folder.
4. Double-click **Fermi.app** to launch it.

> **Requirements:** macOS 13 (Ventura) or later, an internet connection, and a free [Google AI Studio](https://aistudio.google.com/app/apikey) account for your API key.

### ⚠️ macOS says "Fermi can't be opened" or "unidentified developer"

Fermi is distributed independently of the Mac App Store, so Gatekeeper may flag it on first launch. To open it anyway:

1. Right-click (or Control-click) **Fermi.app** and choose **Open**.
2. Click **Open** again in the dialog that appears.

You only need to do this once. If your Mac still blocks it, go to **System Settings → Privacy & Security**, scroll to the Security section, and click **Open Anyway** next to the Fermi message.

---

## First-Launch Setup

The first time you open Fermi, it walks you through a short setup — the same style as macOS's own Setup Assistant:

| Step | What happens |
|---|---|
| **Welcome** | A quick overview of what Fermi does. |
| **System Check** | Confirms you're online and that `git` is installed. If `git` is missing, Fermi will ask permission to install the Xcode Command Line Tools for you. |
| **Terms & License** | Shows the current MIT license for your review. |
| **Download Fermi** | Fetches the Fermi engine into `~/Library/Application Support/Fermi`. |
| **API Key** | Walks you through creating a free key at Google AI Studio and saves it locally to a `.env` file. |
| **Command Line Access** | Optional: adds a `fermi` command to your terminal (`~/.zshrc`) so you can launch it from anywhere. |
| **Finish** | Installs Python dependencies and opens your terminal, ready to go. |

Every launch after that skips straight to a loading screen, silently checks for updates, and opens your terminal session — no re-setup needed.

---

## Using Fermi

Once set up, Fermi opens a Terminal window running its CLI. Just ask a question — Fermi decides on its own how much depth the answer needs, and quietly runs a calculation or a web lookup itself when one would actually help, rather than dumping code on you.

Commands to know (type `/help` any time to see this list in-app):

| Command | Description |
|---|---|
| `/attach <file_path>` | Queue a file (PDF, image, data file) to send with your next message. |
| `/dir <dir_path>` | Queue an entire directory to index into context with your next message. |
| `/clear` | Clear a queued attachment/directory before sending. |
| `/paste` | Enter multi-line input for a long problem statement; finish with a line containing only `/end`. |
| `/copy` | Copy the last response to your clipboard as raw Markdown. |
| `/retry` | Resend your last message (e.g. after switching model or tools). |
| `/history` | Show this session's conversation so far. |
| `/export [path]` | Export this session to a Markdown file. |
| `/sessions` | List all saved sessions. |
| `/switch <name>` | Switch to (or create) another session. |
| `/new <name>` | Start a brand-new session. |
| `/model <flash\|pro>` | Switch between the fast and the deep-reasoning model. |
| `/temp <0.0–1.0>` | Set generation temperature. |
| `/tools` | Show or toggle Fermi's web search / code execution tools. |
| `/persona <text>` | Add a standing instruction for this session (e.g. "focus on E&M", "always show units"). |
| `/help` | Show the command list. |
| `/quit`, `/exit` | Save your session and close. |

Your chosen model, temperature, and tool settings are remembered between launches in a small `config.json` file next to the app data — no need to reconfigure every time.

If you enabled **Command Line Access** during setup, you can also start Fermi any time from any Terminal window by typing:

```bash
fermi
```

---

## How it works

Fermi.app is a native SwiftUI onboarding/launcher for a Python-based CLI engine. On first launch it clones the engine into `~/Library/Application Support/Fermi`; on every later launch it runs `git pull` to keep it current, then hands off to your terminal. Nothing is installed system-wide outside that one folder plus (optionally) a single alias line in your shell profile.

- **This repo** — release builds of `Fermi.app` only.
- **Engine repo** — the Python CLI source Fermi downloads and runs. *(link it here once published)*

---

## FAQ

**Where is my API key stored?**
Locally, in `~/Library/Application Support/Fermi/.env`. Fermi never uploads it anywhere except directly to Google's Gemini API when you ask a question.

**Does Fermi have a Dock icon?**
No — Fermi runs as a lightweight background/agent app. Launch it from `/Applications`, Spotlight, or the `fermi` terminal command if you enabled it.

**I don't have Git installed — do I need to install it myself?**
No. If Fermi doesn't find `git` during setup, it will ask your permission and trigger Apple's own Xcode Command Line Tools installer for you.

**Can I change my API key later?**
Yes — open `~/Library/Application Support/Fermi/.env` in any text editor and replace the value, or delete the file and re-run Fermi's setup by resetting onboarding (see below).

**Is my data used to train anything?**
Fermi only sends your prompts and attachments to Google's Gemini API under your own key, subject to [Google's API terms](https://ai.google.dev/terms). Fermi itself collects nothing.

---

## Uninstalling

1. Quit Fermi and drag `Fermi.app` out of `/Applications` to the Trash.
2. Delete its data folder:
   ```bash
   rm -rf ~/Library/Application\ Support/Fermi
   ```
3. If you enabled the `fermi` command, remove the alias line from `~/.zshrc` (search for the line tagged `# Added by Fermi.app`).

---

## Reporting Issues

Found a bug or have a feature request? Please [open an issue](https://github.com/SourodeepSarkar/fermi/issues) with your macOS version, Fermi version (from **About** or the release tag you downloaded), and steps to reproduce.

## License

Fermi is released under the [MIT License](LICENSE).
