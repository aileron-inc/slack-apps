# withwork マル之進

XTalent Slack 向けのサポート自動返信ボット（Events API の受信側）。返信ロジックは Grok Bot。このディレクトリは Slack App マニフェストとインストール手順だけ。

| 項目 | 値 |
| --- | --- |
| 表示名 | マル之進 |
| Your Apps の表記 | マル之進 (マルノシン) |
| bot `display_name` | `marunoshin` |
| slug | `withwork` |
| App ID | `A0C2DNVFPK8` |
| 設定 | https://api.slack.com/apps/A0C2DNVFPK8 |
| Slack team | XTalent / `TMGCWC4TY` |
| ステータス | keep-active |
| Event Subscriptions path | `/slack/withwork` |
| Request URL | `https://<bypass-worker>/slack/withwork`（Worker デプロイ後に設定。hostname はここに書かない） |
| 台帳 | [APPS.md](../../APPS.md) |

ライブの Event bot は `A0C2DNVFPK8`。マニフェストの表示名は マル之進のまま（コロ助には戻さない）。

| App ID | Your Apps の名前 | 扱い |
| --- | --- | --- |
| `A0C2DNVFPK8` | マル之進 (マルノシン) | keep-active。path `/slack/withwork` |
| `A0C20A95WR5` | withworkコロ助 | 旧表示名の重複。削除候補 |
| `A041U5XSVQQ` | withwork-bot | より古い withwork bot。置き換え済みとみて削除候補 |

Event URL の実値は、この環境では Slack API を呼んで確認していない。

## インストール

1. [Your Apps](https://api.slack.com/apps) → Create New App → From an app manifest
2. ワークスペース **XTalent** (`TMGCWC4TY`) を選ぶ
3. [`manifest.yml`](./manifest.yml) を貼って作成する
4. bypass Worker デプロイ後、Event Subscriptions の Request URL を `https://<bypass-worker>/slack/withwork` にする
5. Install to Workspace
6. Signing Secret をホームディレクトリの secrets へコピーする（下表）
7. `~/.slack-support-bypass/routes.json` に withwork 行を足す（このリポジトリには置かない）
8. 対象チャンネルに マル之進（`@marunoshin`）を invite する

Bot Token (`xoxb-...`) は Grok Bot 側のみ。git に入れない。

## secrets.env のキー

`~/.slack-support-bypass/secrets.env`（bypass Worker が読む。このリポジトリには置かない）:

```
SLACK_SIGNING_SECRET_WITHWORK=
GROK_WEBHOOK_URL_WITHWORK=
GROK_WEBHOOK_KEY_WITHWORK=
```
