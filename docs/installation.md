# 1. Install Hermes

> This is an unofficial guide from REA Research Labs, Inc. It is not affiliated with, sponsored by, or endorsed by Nous Research. "Hermes" refers to the open-source Hermes Agent project by Nous Research ([source](https://github.com/NousResearch/hermes-agent), MIT License).

The installer puts the latest stable Hermes release on your machine. The commands below match the official Hermes [README](https://github.com/NousResearch/hermes-agent/blob/a9ff9799e273f525b82718cdc69fdec71b1dca5a/README.md) and [installation guide](https://hermes-agent.nousresearch.com/docs/getting-started/installation) as of upstream commit `a9ff979` (November 7, 2026). Testing status, limited: on November 7, 2026 we ran only the Linux steps, in a fresh temporary home directory inside a sandbox on one Linux machine (same operating-system user and kernel, not a separate virtual machine), and they installed Hermes v0.23.0 (stable). The macOS and Windows (WSL2) steps are untested by us; they were only checked against the official sources. If this page and the official guide disagree, follow the official guide.

This guide covers macOS, Linux, and Windows through WSL2. Every later step assumes a Linux-style shell and the `~/.hermes/` folder.

## Before you start

- You need a terminal and an internet connection.
- The installer is a script downloaded from the web and run in one step. To read it first, open the [Hermes install script](https://hermes-agent.nousresearch.com/install.sh) in your browser.
- On macOS, the official docs support Apple Silicon. Intel Macs are listed as not supported; see [Platform Support](https://github.com/NousResearch/hermes-agent/blob/a9ff9799e273f525b82718cdc69fdec71b1dca5a/website/docs/getting-started/platform-support.md).

## macOS and Linux

1. Open Terminal.
2. Run the installer:

   ```bash
   curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
   ```

3. Reload your shell. Run the line for your shell (macOS uses zsh by default; most Linux systems use bash):

   ```bash
   source ~/.zshrc
   ```

   ```bash
   source ~/.bashrc
   ```

4. Check the install:

   ```bash
   hermes doctor
   ```

   On a new install, `hermes doctor` usually reports that no provider or API keys are set yet. That is expected; [step 2](first-chat-and-skill.md) fixes it. Look for problems with the install itself, such as a missing `hermes` command.

If the installer starts a setup wizard, you can complete it or exit and follow step 2 instead.

## Windows (WSL2)

1. Open PowerShell as administrator: right-click PowerShell and choose "Run as administrator".
2. Install WSL with Microsoft's command from [Install WSL](https://learn.microsoft.com/windows/wsl/install):

   ```powershell
   wsl --install
   ```

3. Restart Windows when it finishes. Only this step and step 2 need administrator rights.
4. Open Ubuntu (the default distribution) from the Start menu. Create a Linux username and password when asked.
5. In that Ubuntu window, not in PowerShell, run:

   ```bash
   curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
   source ~/.bashrc
   hermes doctor
   ```

From here on, run every command in the Ubuntu window.

If `wsl --install` only prints help text, WSL is already installed; Microsoft's page explains how to add a distribution. Hermes also offers a native Windows install, which this guide does not cover; see the official installation guide.

## Where things live

Your settings, keys file, and skills are in `~/.hermes/`.

## Troubleshooting

| Symptom | Try |
|---|---|
| `hermes: command not found` | Reload your shell (step 3), or open a new terminal |
| `hermes doctor` says provider or API keys are missing | Expected before step 2 |
| `hermes doctor` reports another problem | Follow its message, then see the official installation guide |

To update later:

```bash
hermes update
```

Next: [First chat and first skill](first-chat-and-skill.md)
