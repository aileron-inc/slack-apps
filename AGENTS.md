# Agent boundary: slack-apps

This repository is **Slack App manifests and operator docs only**.

## Do

- Add or update `apps/<slug>/manifest.yml` (Slack app manifest, schema v2).
- Document create / install / secret-placement steps next to that manifest.
- Manage keep-active Events apps as manifests: `apps/sae` (`A0BUXNTB43U`, `/slack/hairbook`), `apps/tsumugi` (`A0BUXEUUPM0`, `/slack/salonjobs`), `apps/hinata` (`A0BUXV96DM0`, `/slack/synsalon`), `apps/withwork` (`A0C2DNVFPK8`, `/slack/withwork`). The inventory is `APPS.md`.
- `apps/withwork-bot` is the XTalent form bot (`A0C84BDDDHN`, `@withwork`). It is not マル之進 and it does not use bypass Event Subscriptions. Do not describe `A041U5XSVQQ` as retired in favor of マル之進.
- Keep the bypass minimum scopes and bot events aligned with [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass):
  - bot scopes: `chat:write`, `app_mentions:read`, `channels:history`, `groups:history`
  - bot events: `app_mention`, `message.channels`, `message.groups`
- An installed app may already have extra scopes (see `apps/tsumugi`). Keep those when the committed manifest mirrors a live export, and document them. Dropping them on re-apply removes permissions. Adding new scopes still needs a reason in the app README.
- `apps/akari` is a retired archive (App `A0BJYNERNG6`, deleted 2026-10-07 JST, successor Sae `A0BUXNTB43U`). The legacy file is `manifest.legacy.yml` on purpose. There is no install `manifest.yml`.

## Do not

- Do not add a Cloudflare Worker, HTTP server, or any long-running daemon. Runtime lives in `aileron-inc/slack-support-bypass`.
- Do not add Grok Bot reply logic, prompts, or webhook handlers.
- Do not commit secrets, tokens, signing secrets, `.env`, `.dev.vars`, or anything matching `*token*` / `*secret*`.
- Do not put `routes.json` here. Operator routes live at `~/.slack-support-bypass/routes.json` (see the bypass repo). This repo must not become a second copy.
- Do not invent a `workers.dev` hostname for `event_subscriptions.request_url`. Leave it unset until the bypass Worker is deployed, then set Request URL in Slack App settings. Document the path only (`/slack/withwork`, `/slack/hairbook`, `/slack/salonjobs`, `/slack/synsalon`).
- Do not call Slack APIs to create or install apps unless a human explicitly asks.
- Do not modify `aileron-inc/slack-support-bypass` from this repo's work unless that is a separate, explicit task.
- Do not paste `apps/akari/manifest.legacy.yml` into Slack, and do not turn Socket Mode on as the Events path.
- Do not recreate the apps that stayed deleted on 2026-10-07 (月島灯, Hairbook Support, the extra 紬 apps, Mia local, Demo App, withworkコロ助), or apps marked keep-elsewhere, personal/demo, or dormant. withwork-bot's old ID `A041U5XSVQQ` was deleted by mistake and recreated as `A0C84BDDDHN`. Do not add manifests for 質問ちゃん or カク之進 unless a human asks. Do not copy the non-git hub at `~/Projects/slack-apps` (icons, secrets symlink, routes) into this repo.

## Secrets stay off git

| Secret | Where it lives |
| --- | --- |
| Slack Signing Secret | `~/.slack-support-bypass/secrets.env` |
| Grok webhook URL / key | `~/.slack-support-bypass/secrets.env` |
| Bot Token (`xoxb-...`) | Grok Bot side only — never here, never in the bypass repo |
