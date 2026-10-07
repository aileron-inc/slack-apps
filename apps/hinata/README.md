# 日向 ひなた｜シンサロンページ

Cajon,Inc. のシンサロンページサポート（Events API）。このディレクトリは Slack App マニフェストとインストール手順だけ。

Your Apps のアプリ名は「日向 ひなた｜シンサロンページ」、App ID は `A0BUXV96DM0`。ローカルハブに App ID は無く、返信実装を `cajon-inc/hairbook-runbook-bot` 系と書いていた。そのリポジトリは Your Apps の行には無い。コードは移さない。

| 項目 | 値 |
| --- | --- |
| 表示名 | 日向 ひなた｜シンサロンページ |
| bot `display_name` | `hinata`（Your Apps はアプリ名のみ。ライブの @mention は未確認） |
| slug | `hinata` |
| App ID | `A0BUXV96DM0` |
| 設定 | https://api.slack.com/apps/A0BUXV96DM0 |
| Slack team | Cajon,Inc. / `T9U503RME` |
| ステータス | keep-active |
| Event Subscriptions path | `/slack/synsalon` |
| Request URL | `https://<bypass-worker>/slack/synsalon`（Worker デプロイ後に設定。hostname はここに書かない） |
| 台帳 | [APPS.md](../../APPS.md) |

ローカルハブのメモはトークンキー `SLACK_BOT_TOKEN_HINATA` と、secrets の場所 `~/Projects/slack-apps/secrets` だけ。追加スコープの記載は無い。[`manifest.yml`](./manifest.yml) は bypass の最小に合わせた。

- bot scopes: `chat:write`, `app_mentions:read`, `channels:history`, `groups:history`
- bot events: `app_mention`, `message.channels`, `message.groups`

## 既存アプリへ再適用する前

対象は `A0BUXV96DM0`。ライブの manifest export は手元に無い。再適用すると、画面側の余分なスコープが落ち、Request URL も外れる。App Manifest 画面と diff してから貼る。

新規作成で Slack UI が Request URL を要求したら、空のまま進める。無理なら Worker デプロイ後に同じマニフェストを再適用する。

## インストール

1. [Your Apps](https://api.slack.com/apps) → Create New App → From an app manifest
2. ワークスペース **Cajon** (`T9U503RME`) を選ぶ
3. [`manifest.yml`](./manifest.yml) を貼って作成する
4. bypass Worker デプロイ後、Event Subscriptions の Request URL を `https://<bypass-worker>/slack/synsalon` にする
5. Install to Workspace
6. Signing Secret をホームディレクトリの secrets へコピーする（下表）
7. `~/.slack-support-bypass/routes.json` に synsalon 行を足す（このリポジトリには置かない）
8. 対象チャンネルに 日向を invite する。manifest の bot `display_name` は `hinata`。ライブの @mention は Your Apps に無い

Bot Token (`xoxb-...`) は Grok Bot 側のみ。git に入れない。

## secrets.env のキー

`~/.slack-support-bypass/secrets.env`（bypass Worker が読む。このリポジトリには置かない）。

Signing Secret と Grok webhook のキー名は withwork の慣例（slug を大文字）に合わせた名前。既存ファイルが別名なら、その名前を維持する。

```
SLACK_SIGNING_SECRET_HINATA=
GROK_WEBHOOK_URL_HINATA=
GROK_WEBHOOK_KEY_HINATA=
```

ローカルハブが記録している Bot token のキー名は `SLACK_BOT_TOKEN_HINATA`。値はこのリポジトリに置かない。
