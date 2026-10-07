# 月島灯（retired）

Hairbook サポートの旧アプリ。Socket Mode。後継は Sae（`A0BUXNTB43U`、Events API、path `/slack/hairbook`）。

| 項目 | 値 |
| --- | --- |
| 表示名 | 月島 灯｜Hairbookサポート |
| bot `display_name` | `akari` |
| slug | `akari` |
| App ID | `A0BJYNERNG6` |
| 設定 | https://api.slack.com/apps/A0BJYNERNG6 |
| Slack team | Cajon / `T9U503RME` |
| ステータス | retired |
| 受信 | Socket Mode（廃止時点の記録） |
| 後継 | [`apps/sae`](../sae/README.md) |
| 台帳 | [APPS.md](../../APPS.md) |

推奨パスは Sae の Events API。bypass Worker（`aileron-inc/slack-support-bypass`）が Request URL で受ける。

[`manifest.legacy.yml`](./manifest.legacy.yml) はローカルハブの export を、コメントを足しただけのアーカイブ。`socket_mode_enabled: true` は廃止時点の設定記録。Create from manifest にも App Manifest エディタにも貼らない。Socket Mode を戻す手順はこのディレクトリに置かない。`manifest.yml` が無いのは、貼るファイルと記録を分けるため。

旧スコープ（`canvases:write`, `channels:join`, `channels:read`, `groups:read`, `users:read`）と、イベントが `message.channels` / `message.groups` だけだったことも、このファイルの記録。Sae 側は bypass の最小スコープに合わせてある。

## 廃止手順（未実施）

このリポジトリの変更では Slack API を呼んでいない。ワークスペースからの削除は Matchy が行う。

1. Sae（`A0BUXNTB43U`）が Hairbook チャンネルのイベントを `/slack/hairbook` で受けていることを確認する。
2. 月島灯を対象チャンネルから外す。
3. Cajon（`T9U503RME`）から App `A0BJYNERNG6` をアンインストールする。
4. Slack のアプリ設定で `A0BJYNERNG6` を削除する。設定 URL は上表。
5. オペレータの `~/.slack-support-bypass/secrets.env` と `routes.json` から月島灯用の行を外す。値もルート表もこのリポジトリには置かない。
6. Bot Token が Grok Bot 側に残っていれば、そちらも外す。

Sae の受信を確認する前に 3 と 4 を進めない。
