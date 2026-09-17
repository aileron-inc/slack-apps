# withwork コロ助

XTalent Slack 向けのサポート自動返信ボット（Events API の受信側）。返信ロジックは Grok Bot。このディレクトリは Slack App マニフェストとインストール手順だけ。

| 項目 | 値 |
| --- | --- |
| 表示名 | コロ助 / withworkコロ助 |
| slug | `withwork` |
| Slack team | XTalent / `TMGCWC4TY` |
| Event Subscriptions path | `/slack/withwork` |
| Request URL | `https://<bypass-worker>/slack/withwork`（Worker デプロイ後に設定。hostname はここに書かない） |

## インストール

1. [Your Apps](https://api.slack.com/apps) → Create New App → From an app manifest
2. ワークスペース **XTalent** (`TMGCWC4TY`) を選ぶ
3. [`manifest.yml`](./manifest.yml) を貼って作成する
4. bypass Worker デプロイ後、Event Subscriptions の Request URL を `https://<bypass-worker>/slack/withwork` にする
5. Install to Workspace
6. Signing Secret をホームディレクトリの secrets へコピーする（下表）
7. `~/.slack-support-bypass/routes.json` に withwork 行を足す（このリポジトリには置かない）
8. 対象チャンネルに コロ助 を invite する

Bot Token (`xoxb-...`) は Grok Bot 側のみ。git に入れない。

## secrets.env のキー

`~/.slack-support-bypass/secrets.env`（bypass Worker が読む。このリポジトリには置かない）:

```
SLACK_SIGNING_SECRET_WITHWORK=
GROK_WEBHOOK_URL_WITHWORK=
GROK_WEBHOOK_KEY_WITHWORK=
```
