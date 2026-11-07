# 3. Safety basics

> This is an unofficial community guide from REA Research Labs, Inc. It is not affiliated with, sponsored by, or endorsed by Nous Research. "Hermes" refers to the open-source Hermes Agent project by Nous Research ([source](https://github.com/NousResearch/hermes-agent), MIT License).

An agent that can run commands acts with your user account's permissions. The settings below reduce risk; none of them makes an agent safe by itself. Details are on the official [Security page](https://hermes-agent.nousresearch.com/docs/user-guide/security).

## Keep approvals on

Hermes asks before running commands it treats as dangerous. Check the current mode:

```bash
hermes config get approvals.mode
```

A new install reports `smart`. To be asked every time, switch to `manual`:

```bash
hermes config set approvals.mode manual
```

- The documented modes are `smart`, `manual`, and `off`.
- Do not use `off`, `--yolo`, `/yolo`, or `HERMES_YOLO_MODE=1`. They skip approval prompts.
- `approvals.deny` lets you add your own blocked command patterns; see the Security page.

Approvals cover the actions Hermes classifies as dangerous. They are not a full review of everything the agent does.

## Turn on only the tools you need

```bash
hermes tools
```

Turn off tool groups you are not using yet, such as browser or messaging. Fewer tools leave fewer ways for a bad instruction to cause harm. The official docs also describe running tools in a container such as Docker.

## Treat outside content as untrusted

Web pages, emails, files, and tool output can contain instructions written by someone else. Review before letting the agent act on them, especially for sending messages, spending money, or deleting files.

## Protect secrets

- Keep keys in `~/.hermes/.env`, not in chats, skills, or repositories.
- Give the agent its own key, with a spending limit set in your provider's console if the provider offers one.
- If a key shows up anywhere public, revoke it at the provider and create a new one.

## Stopping and cost

- `Ctrl+C` interrupts the current turn in the terminal. Check afterward that nothing was left half-done.
- If you started the messaging gateway or scheduled jobs, they run separately from your chat; see the official docs for stopping them.
- Most providers charge per use. `/usage` shows the session's usage in Hermes; your provider's billing page is the authoritative record of what you are charged.

Next: [Optional: audit and policy with REA ControlPlane](rea-controlplane.md)
