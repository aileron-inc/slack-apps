# さえ｜Hairbookサポート

Cajon,Inc. の Hairbook サポート（Events API）。返信ロジックは Grok Bot。このディレクトリは Slack App マニフェストとインストール手順だけ。

Your Apps の表示名は「さえ｜Hairbookサポート」。ローカルハブの見出しは "Sae" だった。マニフェストの `display_information.name` は Your Apps に合わせる。

月島灯（`A0BJYNERNG6`、Socket Mode）の後継。その Slack アプリと、英語名 Hairbook Support（`A0BUVGCE47Q`）は 2026-10-07（JST）に削除済み。記録は [`apps/akari`](../akari/README.md)。このディレクトリの対象は `A0BUXNTB43U`。

| 項目 | 値 |
| --- | --- |
| 表示名 | さえ｜Hairbookサポート |
| bot `display_name` / @mention | `hairbook_support` |
| slug | `sae` |
| App ID | `A0BUXNTB43U` |
| 設定 | https://api.slack.com/apps/A0BUXNTB43U |
| Slack team | Cajon,Inc. / `T9U503RME` |
| ステータス | keep-active |
| Event Subscriptions path | `/slack/hairbook` |
| Request URL | `https://<bypass-worker>/slack/hairbook`（Worker デプロイ後に設定。hostname はここに書かない） |
| 台帳 | [APPS.md](../../APPS.md) |

ローカルハブ（`~/Projects/slack-apps`、git ではない）の 2026-09-17 メモ:

- OAuth 再インストール済み。`auth.test` ok（bot user: `hairbook_support`）。同じハブ README の Bot 一覧は「再インストール待ち」と書いてある。個別メモと、ハブ README 末尾のステータス節（再インストール済み）を採用した
- アイコンファイルはハブ側の `assets/sae-app-icon-512.png`。このリポジトリには置かない
- 完全な manifest はハブに無かった。[`manifest.yml`](./manifest.yml) は bypass の最小スコープとイベントで作った

メモは追加スコープを要求していない。bypass が転送に使うのは次だけ。

- bot scopes: `chat:write`, `app_mentions:read`, `channels:history`, `groups:history`
- bot events: `app_mention`, `message.channels`, `message.groups`

## 既存アプリへ再適用する前

`A0BUXNTB43U` の App Manifest 画面とこのファイルを diff する。画面側に余分なスコープやイベントがある状態でこのファイルを再適用すると、その権限は落ちる。Request URL もマニフェストに無いので、再適用のあと `https://<bypass-worker>/slack/hairbook` を入れ直す。

新規作成なら、Create from manifest の時点で Slack UI が Request URL を要求することがある。空のまま進める。無理なら Worker デプロイ後に同じマニフェストを再適用する。

## インストール

1. [Your Apps](https://api.slack.com/apps) → Create New App → From an app manifest
2. ワークスペース **Cajon** (`T9U503RME`) を選ぶ
3. [`manifest.yml`](./manifest.yml) を貼って作成する
4. bypass Worker デプロイ後、Event Subscriptions の Request URL を `https://<bypass-worker>/slack/hairbook` にする
5. Install to Workspace
6. Signing Secret をホームディレクトリの secrets へコピーする（下表）
7. `~/.slack-support-bypass/routes.json` に hairbook 行を足す（このリポジトリには置かない）
8. 対象チャンネルに `@hairbook_support` を invite する

Bot Token (`xoxb-...`) は Grok Bot 側のみ。git に入れない。

## secrets.env のキー

`~/.slack-support-bypass/secrets.env`（bypass Worker が読む。このリポジトリには置かない）。

Signing Secret と Grok webhook のキー名は withwork の慣例（slug を大文字）に合わせた名前。既存ファイルが別名なら、その名前を維持する。

```
SLACK_SIGNING_SECRET_SAE=
GROK_WEBHOOK_URL_SAE=
GROK_WEBHOOK_KEY_SAE=
```

ローカルハブが記録している Bot token のキー名は `SLACK_BOT_TOKEN_SAE`。メモ上の置き場は `~/.slack-support-bypass/secrets.env`、暗号化ストア `~/Projects/hairbook-runbook-secrets/slack_bots.env`、および CF Worker secret。値はこのリポジトリに置かない。
