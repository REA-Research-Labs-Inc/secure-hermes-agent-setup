# 2. Connect a model, chat, and add a skill

> This is an unofficial guide from REA Research Labs, Inc. It is not affiliated with, sponsored by, or endorsed by Nous Research. "Hermes" refers to the open-source Hermes Agent project by Nous Research ([source](https://github.com/NousResearch/hermes-agent), MIT License).

## Pick a model provider

Hermes does not include a model. You connect a provider account. Run the interactive picker:

```bash
hermes model
```

It asks for the provider, the model, and a sign-in or API key. `hermes setup` runs the full wizard instead. Providers are listed on the official [AI Providers](https://hermes-agent.nousresearch.com/docs/integrations/providers) page.

If you paste an API key, the picker stores it for you. If you add one by hand, put it in `~/.hermes/.env`, using the variable name the providers page gives for your provider. The line looks like this, with your real values in place of the placeholders:

```bash
# ~/.hermes/.env
PROVIDER_KEY_VARIABLE_NAME=your-key-here
```

Never paste a key into a chat, a screenshot, or a public issue.

## First chat

```bash
hermes
```

Send a short message, for example: "Say hello in one sentence." Useful commands inside a chat:

| Command | What it does |
|---|---|
| `/new` | Start a fresh conversation |
| `/model` | Switch model |
| `/usage` | Show usage for the session |
| `Ctrl+C` | Interrupt the current turn |

## Before your first skill: check approvals

Hermes asks before running commands it treats as dangerous. Before you add skills, confirm approvals are on:

```bash
hermes config get approvals.mode
```

A new install reports `smart`. To require approval for actions Hermes treats as dangerous, switch to `manual`:

```bash
hermes config set approvals.mode manual
```

[Safety basics](safety.md) (the next step) explains these modes and the other safety settings.

## A first skill

A skill is a folder holding a `SKILL.md` file of instructions. Your own skills go in `~/.hermes/skills/`. The format is on the official [Skills page](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills).

1. Create the skill:

   ```bash
   mkdir -p ~/.hermes/skills/daily-plan
   cat > ~/.hermes/skills/daily-plan/SKILL.md <<'SKILL'
   ---
   name: daily-plan
   description: Turn a list of tasks into a short, ordered plan for today
   version: 1.0.0
   ---

   # Daily plan

   When the user shares tasks for the day:

   1. Group them into Must do, Should do, and Later.
   2. Put at most three items in Must do.
   3. Reply in plain text. Do not run commands, open files, or browse.
   SKILL
   ```

2. Confirm Hermes lists it:

   ```bash
   hermes skills list
   ```

3. Start a chat with `hermes` and call the skill by name, followed by your request:

   ```text
   /daily-plan email Sam, fix bike, pay rent, read chapter 4
   ```

   If a chat was already open when you created the skill, start a new one with `hermes` so the command is picked up.

A skill is guidance for the model, not a permission setting. The "do not run commands" line asks the model to stay in plain text; your approval settings and enabled tools are what actually limit it (see [Safety basics](safety.md)). Hermes can also write skills itself; read any skill before you rely on it.

## Troubleshooting

| Symptom | Try |
|---|---|
| Empty replies or auth errors | Run `hermes model` again, then `hermes doctor` |
| `/daily-plan` not recognized | Check `hermes skills list`, confirm the folder name and the `name:` line match, and start a new chat |

Next: [Safety basics](safety.md)
