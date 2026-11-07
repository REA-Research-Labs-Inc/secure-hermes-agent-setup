# 4. Optional: audit and policy with REA ControlPlane

> This is an unofficial community guide from REA Research Labs, Inc. It is not affiliated with, sponsored by, or endorsed by Nous Research. "Hermes" refers to the open-source Hermes Agent project by Nous Research ([source](https://github.com/NousResearch/hermes-agent), MIT License).

REA ControlPlane sits on the model hop. Hermes sends its model requests to REA, and REA forwards them to your model vendor using your own vendor key. LoopGuard, PreFlight, and TraceGuard are included on every plan. Which checks act on a request depends on its contents and the controls configured for your key. REA does not host the model.

REA does not sandbox Hermes on your computer. Commands, file edits, and other local tools still run under your account, governed by Hermes' own approvals. Keep [Safety basics](safety.md) in place.

REA's public pages: [Hermes setup](https://reacontrolplane.com/agents/hermes.html) and [Set up](https://reacontrolplane.com/setup.html).

## What you need

- A REA API key. You get one when you create an account through REA's [sign-up page](https://api.reacontrolplane.com/signup); it is shown once, so store it safely.
- An API key from your model vendor's developer console. A consumer chat-app sign-in or subscription does not work through REA; you need an API key.
- Hermes installed, with a first chat working ([step 2](first-chat-and-skill.md)) and [Safety basics](safety.md) (step 3) in place.

## 1. Add both keys to Hermes' env file

Open `~/.hermes/.env` in a text editor. The installer creates it with commented examples. Add these two lines at the end, with your real keys in place of the placeholders:

```bash
REA_API_KEY=your-rea-key
UPSTREAM_MODEL_KEY=your-vendor-api-key
```

These variable names are this guide's choice. The config below refers to them by name, so the keys never appear in `config.yaml`.

## 2. Add REA as a custom provider

First, add this block at the end of `~/.hermes/config.yaml`. Paste it as is; it contains no keys.

```yaml
providers:
  rea:
    api: https://api.reacontrolplane.com/v1
    key_env: REA_API_KEY
    transport: chat_completions
    extra_headers:
      X-Upstream-Api-Key: ${UPSTREAM_MODEL_KEY}
```

If the file already has a top-level `providers:` line, add only the `rea:` part under it instead.

Then select it as your model provider. Do not paste a second `model:` section; the file already has one. Use these commands:

```bash
hermes config set model.provider custom:rea
hermes config set model.default YOUR_MODEL_ID
```

What each line does:

- `api`: REA's OpenAI-compatible address. It ends in `/v1`.
- `key_env`: the variable holding your REA key, sent as the API key.
- `transport: chat_completions`: use the OpenAI Chat Completions format.
- `extra_headers`: sends your vendor key to REA in the `X-Upstream-Api-Key` header. `${UPSTREAM_MODEL_KEY}` is filled in from your env file.
- `model.provider` set to `custom:rea` selects this provider; `model.default` is the model.

Replace `YOUR_MODEL_ID` with a model ID from your vendor that REA's [Set up](https://reacontrolplane.com/setup.html) page lists. For a direct vendor, use the ID exactly as the vendor publishes it, for example `gpt-4o-mini` with an OpenAI key. Some routed hosts need a prefix in front of the ID; the Set up page shows it. Model availability changes, so check the current lists before you copy an ID.

Hermes documents these settings on its [AI Providers](https://hermes-agent.nousresearch.com/docs/integrations/providers) and [Configuration](https://hermes-agent.nousresearch.com/docs/user-guide/configuration) pages.

### Alternative: one combined key

REA also accepts the REA key and vendor key joined by a dot, for tools with no header setting. Use one method only. If you set both, REA forwards the combined key.

```bash
REA_COMBINED_KEY=your-rea-key.your-vendor-api-key
```

Then change `key_env` to `REA_COMBINED_KEY` and delete the two `extra_headers` lines.

## What we tested

This is a limited result. On November 7, 2026, on Linux with Hermes v0.23.0, we followed the steps above with an OpenAI API key and the model `gpt-4o-mini`, then sent three short one-off messages with `hermes chat -q`, with tools disabled. All three got normal replies through the REA model hop. That shows the route and the keys worked for those calls; it does not show which REA controls acted on them.

The test requests above were manually confirmed in the console by the account owner.

Not tested: an interactive session, agent tools, the combined-key method, macOS, Windows (WSL2), and other vendors or models.

## 3. Check it works

1. Start a new chat and send one short message:

   ```bash
   hermes
   ```

2. You should get a normal reply.
3. Sign in to the [REA console](https://api.reacontrolplane.com/app) with the email and password from account creation. Choose Run summary, find your request in Turns, and select its row. In the Turn inspector, under Usage, check Input tokens and Output tokens. These steps follow the console's public sign-in page.

## Troubleshooting

| Symptom | Things to check |
|---|---|
| 401 or other auth error | The REA key is in `~/.hermes/.env` under the name given in `key_env`, with no extra spaces or quotes |
| Vendor auth error | The vendor key belongs to the vendor that serves your model. REA does not swap keys |
| Model not found | The model ID matches the vendor's published ID and REA's Set up page |
| Vendor key error mentioning `${UPSTREAM_MODEL_KEY}` | The variable is missing from `~/.hermes/.env`. Hermes leaves an unset `${...}` reference as plain text |
| Reply works but no turn in the console | Give it a moment and refresh. Check you are signed in to the account that owns the REA key and that no filter hides the turn. Run `hermes config get model.provider` to confirm it prints `custom:rea`, and `hermes doctor` for config errors. If it still does not appear, contact REA support with the time of the request, not your keys |

To stop using REA, run `hermes model` and choose your previous provider.
