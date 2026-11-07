# Get your keys

> This is an unofficial guide from REA Research Labs, Inc. It is not affiliated with, sponsored by, or endorsed by Nous Research, OpenAI, Anthropic, or xAI. "Hermes" refers to the open-source Hermes Agent project by Nous Research ([source](https://github.com/NousResearch/hermes-agent), MIT License).

Do this before [step 2](first-chat-and-skill.md).

## What an API key is

An API key is a long secret string that lets a program use your account. Anyone who has it can run up charges on your account, so treat it like a password.

## What you need

- **One model vendor key.** Pick one vendor below. You don't need more than one.
- **A REA key, only if you want the optional REA step** ([step 4](rea-controlplane.md)). For that step you need both a REA key and a vendor key. If you skip REA, you only need the vendor key.

A chat subscription is not an API key. ChatGPT and Claude subscriptions are billed separately from API use, and they don't give you a key for Hermes. API use is paid through the vendor's developer console, usually with prepaid credits.

## Get a model vendor key (pick one)

These steps come from each vendor's public documentation, checked on 7 November 2026. We did not create accounts or keys to test them, and page and button names may change. If something looks different, follow the linked official page.

### OpenAI

1. Sign in at the [OpenAI API platform](https://platform.openai.com). This is a different account area from ChatGPT ([billing is separate](https://help.openai.com/en/articles/9039756-billing-settings-in-chatgpt-vs-platform)).
2. Add a payment method and buy credits in the API billing settings. New API accounts use [prepaid billing](https://help.openai.com/en/articles/8264644-how-can-i-set-up-prepaid-billing). Auto-reload is on by default during setup. Turn it off if you don't want automatic purchases.
3. Create a key on the [API keys page](https://platform.openai.com/api-keys) ([help](https://help.openai.com/en/articles/4936850-where-do-i-find-my-openai-api-key)). OpenAI shows the full key only once, when you create it. Copy it straight into a password manager.

### Anthropic (Claude)

1. Create or sign in to a [Claude Console](https://platform.claude.com) account. This is separate from a Claude chat plan ([billing is separate](https://support.anthropic.com/en/articles/9876003-i-have-a-paid-claude-subscription-pro-max-team-or-enterprise-plans-why-do-i-have-to-pay-separately-to-use-the-claude-api-and-console)).
2. Set up billing in the Console.
3. Create a key on the API keys page in the Console ([getting started](https://docs.anthropic.com/en/api/getting-started)). Copy it into a password manager right away.

### xAI

1. Sign up at the [xAI Console](https://console.x.ai) and add credits ([quickstart](https://docs.x.ai/docs/tutorial), [billing](https://docs.x.ai/docs/key-information/billing)).
2. Create a key on the API Keys page in the Console. Copy it into a password manager right away.

For Anthropic and xAI, we did not find a public page saying whether you can view the key again later. Assume you can't.

## Get a REA key (optional)

1. Open REA's [sign-up page](https://api.reacontrolplane.com/signup) and create an account.
2. The sign-up page says the next step saves a card and lets you start a 7-day trial or start a plan.
3. The trial key is shown once on that page. Copy it into a password manager before you leave the page.

## Keep spending small

- Start with a small credit purchase and leave automatic top-ups off, or give them a low monthly maximum.
- Know whether a limit actually stops requests or only sends an alert:
  - **OpenAI:** A [project spend limit](https://help.openai.com/en/articles/9186755-managing-your-work-in-the-api-platform-with-projects) only monitors spending unless you turn on "Enforce a hard limit". Even then, spending can go slightly over the limit before requests stop. Spend alerts send a notification and don't stop requests. When prepaid credits run out, requests fail, but there can be a short delay first.
  - **Anthropic:** You can set your own [spend limit](https://docs.anthropic.com/en/api/rate-limits) on the Console Billing page. When usage reaches it, requests are refused.
  - **xAI:** With the default $0 invoiced billing limit, requests are refused once prepaid credits run out ([billing](https://docs.x.ai/docs/key-information/billing)). Auto top-up has a monthly maximum you can set. The 80% warning is a notice only.
- None of these is an instant cutoff. Check your usage page after your first few chats.

## Store keys safely

- Keep a backup in a password manager. For Hermes, put the key in your local `~/.hermes/.env` file after installation, outside any project or repository. Use placeholders in shared examples.
- Never paste a key into a chat (including a chat with Hermes), a screenshot, an issue, or a git repository.
- After installing Hermes and once that file exists, make it readable only by you:

  ```bash
  chmod 600 ~/.hermes/.env
  ```

- Add keys with a text editor, or by pasting into `hermes model` when it asks. Don't type them into commands like `echo` or `export`, because your shell can save them in its history.

## If a key leaks or you lose it

**Vendor key leaked:** revoke it right away in that vendor's console, then create a new key and put it in `~/.hermes/.env` in place of the old one. Anything still using the old key stops working.

**Vendor key lost (not leaked):** create a new key in the same console, replace it in `~/.hermes/.env`, then delete the old key.

**REA key leaked or lost:** contact REA support through [reacontrolplane.com](https://reacontrolplane.com). Do not include the key itself in your message. Remove the old key from `~/.hermes/.env` until you have a replacement.

Next: [Install Hermes](installation.md), if you haven't yet, then [connect a model](first-chat-and-skill.md).
