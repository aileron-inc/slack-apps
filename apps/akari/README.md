# 月島灯（retired）

Hairbook サポートの旧アプリ。Socket Mode。後継は さえ｜Hairbookサポート（`A0BUXNTB43U`、Events API、path `/slack/hairbook`）。

| 項目 | 値 |
| --- | --- |
| 表示名 | 月島 灯｜Hairbookサポート |
| bot `display_name` | `akari` |
| slug | `akari` |
| App ID | `A0BJYNERNG6` |
| 設定 | https://api.slack.com/apps/A0BJYNERNG6 （削除前の URL） |
| Slack team | Cajon,Inc. / `T9U503RME` |
| ステータス | retired。Slack アプリは 2026-10-07（JST）に削除済み |
| 受信 | Socket Mode（廃止時点の記録） |
| 後継 | [`apps/sae`](../sae/README.md) |
| 台帳 | [APPS.md](../../APPS.md) |

推奨パスは さえ（`A0BUXNTB43U`）の Events API。bypass Worker（`aileron-inc/slack-support-bypass`）が Request URL で受ける。

[`manifest.legacy.yml`](./manifest.legacy.yml) はローカルハブの export を、コメントを足しただけのアーカイブ。`socket_mode_enabled: true` は廃止時点の設定記録。Create from manifest にも App Manifest エディタにも貼らない。Socket Mode を戻す手順はこのディレクトリに置かない。`manifest.yml` が無いのは、貼るファイルと記録を分けるため。

旧スコープ（`canvases:write`, `channels:join`, `channels:read`, `groups:read`, `users:read`）と、イベントが `message.channels` / `message.groups` だけだったことも、このファイルの記録。さえ側は bypass の最小スコープに合わせてある。

## 削除（2026-10-07）

Slack アプリ `A0BJYNERNG6` は 2026-10-07（JST）に `slack app delete --force` で削除済み。このディレクトリは Socket Mode 時代の記録として残す。

`~/.slack-support-bypass/secrets.env` と `routes.json`、Grok Bot 側の Bot Token から月島灯の行を外したかは、このフォローでは未確認。値はここに書かない。
