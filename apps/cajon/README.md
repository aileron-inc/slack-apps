# cajon カクノシン

Cajon,Inc. Slack 向け。開発・広告まわりの未対応と要対応をまっちに届け、依頼があれば伴走する（Events API の受信側）。返信ロジックは Grok Bot。このディレクトリは Slack App マニフェストとインストール手順だけ。

| 項目 | 値 |
| --- | --- |
| 表示名 | カクノシン |
| slug | `cajon` |
| Slack team | Cajon,Inc. / `T9U503RME` |
| Event Subscriptions path | `/slack/cajon` |
| Request URL | `https://<bypass-worker>/slack/cajon`（Worker デプロイ後に設定。hostname はここに書かない） |

## インストール

1. [Your Apps](https://api.slack.com/apps) → Create New App → From an app manifest
2. ワークスペース **Cajon,Inc.** (`T9U503RME`) を選ぶ
3. [`manifest.yml`](./manifest.yml) を貼って作成する
4. bypass Worker デプロイ後、Event Subscriptions の Request URL を `https://<bypass-worker>/slack/cajon` にする
5. Install to Workspace
6. Signing Secret をホームディレクトリの secrets へコピーする（下表）
7. `~/.slack-support-bypass/routes.json` に cajon 行を足す（このリポジトリには置かない）
8. 対象チャンネルに カクノシン を入れる

`channels:join` は公開チャンネルのみ。プライベートチャンネル（例: `#dev_hairbook_dept`, `#広告運用チーム`）はワークスペースメンバーが `/invite @kakunoshin` する。

Bot Token (`xoxb-...`) は Grok Bot 側のみ。git に入れない。

## secrets.env のキー

`~/.slack-support-bypass/secrets.env`（bypass Worker が読む。このリポジトリには置かない）:

```
SLACK_SIGNING_SECRET_CAJON=
GROK_WEBHOOK_URL_CAJON=
GROK_WEBHOOK_KEY_CAJON=
```
