# withwork-bot

XTalent のフォーム / アンケート用 Slack bot（`@withwork`）。マル之進とは別システム。

マル之進（`A0C2DNVFPK8`、[`apps/withwork`](../withwork/README.md)）はサポートの Events API で、bypass path は `/slack/withwork`、シークレットのキーは `*_WITHWORK`。このディレクトリのアプリはそこへイベントを送らない。

| 項目 | 値 |
| --- | --- |
| 表示名 | withwork-bot |
| bot | `@withwork` |
| App ID | `A0C84BDDDHN` |
| bot_id | `B0C6UJMAR2B` |
| user | `U0C781KG92A` |
| 設定 | https://api.slack.com/apps/A0C84BDDDHN |
| Slack team | XTalent / `TMGCWC4TY` |
| ステータス | product-bot（keep-active の Events 表には入れない） |
| Event Subscriptions | なし（bypass に向けない） |
| Interactivity | ON。本番 URL は既存の `https://withwork.com/webhook/slack/...` |
| ランタイム | CF Worker `withwork-slack-sync` と Rails の `SLACK_API_TOKEN` |
| 台帳 | [APPS.md](../../APPS.md) |

2026-10-07（JST）に旧アプリを誤って削除し、同日 XTalent で再作成した。

| | 旧（履歴） | 現行 |
| --- | --- | --- |
| App ID | `A041U5XSVQQ` | `A0C84BDDDHN` |
| bot_id | `B048NKTU1JL` | `B0C6UJMAR2B` |
| user | `U047W0N0DPX` | `U0C781KG92A` |

旧 ID は Deleted 2026-10-07 の記録。retire-candidate ではなく、マル之進へ置き換えたわけでもない。

## スコープ

次は 2026-10-07 のオーナー指定からの推定で、ライブ export ではない。

- `chat:write`
- `channels:history`, `channels:read`
- `groups:history`, `groups:read`
- `users:read`
- `usergroups:read`

## Interactivity

Interactivity は ON。Request URL は本番で使っている `https://withwork.com/webhook/slack/...`。パスの残りはこのリポジトリに無いので、[`manifest.yml`](./manifest.yml) の `request_url` は空にしてある。続きを推測して書かない。

このファイルを `A0C84BDDDHN` に再適用すると、画面上の Interactivity URL が外れ、スコープもこの推定リストに置き換わる。貼る前に App Manifest 画面と diff する。

## シークレット

Bot Token と Signing Secret の値は git に入れない。Rails 側の環境変数名は `SLACK_API_TOKEN`。bypass 用の `*_WITHWORK` とは別。
