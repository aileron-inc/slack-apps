# Agent boundary: slack-apps

This repository is **Slack App manifests and operator docs only**.

## Do

- Add or update `apps/<slug>/manifest.yml` (Slack app manifest, schema v2).
- Document create / install / secret-placement steps next to that manifest.
- Keep scopes and bot events aligned with [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass):
  - bot scopes: `chat:write`, `app_mentions:read`, `channels:history`, `groups:history`
  - bot events: `app_mention`, `message.channels`, `message.groups`

## Do not

- Do not add a Cloudflare Worker, HTTP server, or any long-running daemon. Runtime lives in `aileron-inc/slack-support-bypass`.
- Do not add Grok Bot reply logic, prompts, or webhook handlers.
- Do not commit secrets, tokens, signing secrets, `.env`, `.dev.vars`, or anything matching `*token*` / `*secret*`.
- Do not put `routes.json` here. Operator routes live at `~/.slack-support-bypass/routes.json` (see the bypass repo). This repo must not become a second copy.
- Do not invent a `workers.dev` hostname for `event_subscriptions.request_url`. Leave it unset until the bypass Worker is deployed, then set Request URL in Slack App settings.
- Do not call Slack APIs to create or install apps unless a human explicitly asks.
- Do not modify `aileron-inc/slack-support-bypass` from this repo's work unless that is a separate, explicit task.

## Secrets stay off git

| Secret | Where it lives |
| --- | --- |
| Slack Signing Secret | `~/.slack-support-bypass/secrets.env` |
| Grok webhook URL / key | `~/.slack-support-bypass/secrets.env` |
| Bot Token (`xoxb-...`) | Grok Bot side only — never here, never in the bypass repo |
