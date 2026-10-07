# 森崎 紬｜サロンジョブズ

Cajon Slack のサロンジョブズサポート（Events API）。返信ロジックは Grok Bot。このディレクトリは Slack App マニフェストとインストール手順だけ。

| 項目 | 値 |
| --- | --- |
| 表示名 | 森崎 紬｜サロンジョブズ |
| bot `display_name` | `tsumugi` |
| slug | `tsumugi` |
| App ID | `A0BUXEUUPM0`（Your Apps の本番。export ファイル自体には App ID が無い） |
| 設定 | https://api.slack.com/apps/A0BUXEUUPM0 |
| Slack team | Cajon,Inc. / `T9U503RME` |
| ステータス | keep-active |
| Event Subscriptions path | `/slack/salonjobs` |
| Request URL | `https://<bypass-worker>/slack/salonjobs`（Worker デプロイ後に設定。hostname はここに書かない） |
| 台帳 | [APPS.md](../../APPS.md) |

Your Apps に同じ表示名が複数ある。このディレクトリが追うのは本番の `A0BUXEUUPM0` だけ。

| App ID | Your Apps の名前 | 扱い |
| --- | --- | --- |
| `A0BUXEUUPM0` | 森崎 紬｜サロンジョブズ | keep-active。bypass path `/slack/salonjobs` |
| `A0BUGFXQNDD` | 森崎 紬｜サロンジョブズ | 同名の別アプリ。削除候補。Event URL は未確認（Slack API は呼んでいない） |
| `A0BUR8478TV` | 森崎 紬｜サロンジョブズ (local) | local の重複。削除候補 |

[`manifest.yml`](./manifest.yml) はローカルハブ（`~/Projects/slack-apps`、git ではない）の export が元。yaml と json の内容は一致していた。コミット時に外したものは `event_subscriptions.request_url` だけ（export には `workers.dev` のホストが入っていた）。どの App ID の export かはファイルに書いていない。Your Apps の分類では本番は `A0BUXEUUPM0`。

## スコープ

bypass Worker の転送に必要な最小は `chat:write`, `app_mentions:read`, `channels:history`, `groups:history` と、イベント `app_mention`, `message.channels`, `message.groups`。

export には次も入っていた。メモは各スコープが必須である理由を書いていない。再適用で落とすと、インストール済みの権限が減る。狭めるかは Matchy の判断にするまで、export どおり残す。

| scope | 区分 |
| --- | --- |
| `app_mentions:read`, `channels:history`, `chat:write`, `groups:history` | bypass の最小 |
| `channels:join`, `channels:read`, `files:read`, `groups:read`, `reactions:write`, `users:read` | export に入っていた追加分。必須理由はメモに無い |

## 既存アプリへ再適用する前

再適用すると Request URL がマニフェストから外れた状態になる。適用後に `https://<bypass-worker>/slack/salonjobs` を入れ直す。

新規作成で Slack UI が Request URL を要求したら、空のまま進める。無理なら Worker デプロイ後に同じマニフェストを再適用する。

## インストール

1. [Your Apps](https://api.slack.com/apps) → Create New App → From an app manifest
2. ワークスペース **Cajon** (`T9U503RME`) を選ぶ
3. [`manifest.yml`](./manifest.yml) を貼って作成する
4. bypass Worker デプロイ後、Event Subscriptions の Request URL を `https://<bypass-worker>/slack/salonjobs` にする
5. Install to Workspace
6. Signing Secret をホームディレクトリの secrets へコピーする（下表）
7. `~/.slack-support-bypass/routes.json` に salonjobs 行を足す（このリポジトリには置かない）
8. 対象チャンネルに `@tsumugi` を invite する

Bot Token (`xoxb-...`) は Grok Bot 側のみ。git に入れない。

## secrets.env のキー

`~/.slack-support-bypass/secrets.env`（bypass Worker が読む。このリポジトリには置かない）。

Signing Secret と Grok webhook のキー名は withwork の慣例（slug を大文字）に合わせた名前。既存ファイルが別名なら、その名前を維持する。ローカルハブの README はトークンの環境変数名を書いておらず、`apps/tsumugi/.env` と secrets を指しているだけ。

```
SLACK_SIGNING_SECRET_TSUMUGI=
GROK_WEBHOOK_URL_TSUMUGI=
GROK_WEBHOOK_KEY_TSUMUGI=
```
