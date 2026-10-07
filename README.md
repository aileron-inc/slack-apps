# slack-apps

Matchy の Slack App 管理リポジトリ。**マニフェストと作成・インストール手順だけ**を置く。

これは次のものではない。

- Events 転送 Worker（[aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass)）
- Grok Bot 側の返信ロジック

ランタイムもデーモンもここには置かない。シークレットも git に入れない。

## レイアウト

```
APPS.md                    # 関連アプリとローカル重複の台帳
apps/<slug>/manifest.yml   # Slack app manifest (schema v2)
apps/<slug>/README.md      # そのアプリのインストール手順
apps/akari/manifest.legacy.yml  # 月島灯の Socket Mode 記録。貼らない
apps/withwork-bot/          # フォーム bot。Events → bypass ではない
```

台帳は [APPS.md](./APPS.md)。Events → bypass のマニフェストは keep-active の 4 つ。`apps/withwork`（マル之進 `A0C2DNVFPK8` / XTalent）、Cajon,Inc. の `apps/sae`（さえ `A0BUXNTB43U`）、`apps/tsumugi`（`A0BUXEUUPM0`）、`apps/hinata`（`A0BUXV96DM0`）。この 4 件は 2026-10-07 の削除に入っていない。`apps/withwork-bot` は XTalent のフォーム bot（`A0C84BDDDHN`、`@withwork`）で、マル之進とは別。旧 ID `A041U5XSVQQ` は同日の誤削除。`apps/akari` は月島灯の Socket Mode 記録。Slack アプリ `A0BJYNERNG6` は 2026-10-07（JST）に削除済み。同日の削除一覧は APPS.md の Deleted 2026-10-07。

## 境界

| 置く場所 | 内容 |
| --- | --- |
| このリポジトリ | マニフェスト、作成・インストール手順 |
| `aileron-inc/slack-support-bypass` | Slack Events → Grok webhook の薄い Worker |
| `~/.slack-support-bypass/routes.json` | オペレータのルート表（git に置かない） |
| `~/.slack-support-bypass/secrets.env` | Signing Secret と Grok webhook 認証（git に置かない） |
| Grok Bot 側 | Bot Token (`xoxb-...`) と返信ロジック |

`routes.json` をこのリポジトリにコピーしないこと。ホームディレクトリと bypass リポジトリの関心事。

## bypass Worker と揃えるスコープ / イベント

マニフェストは bypass Worker が検証・転送する前提に合わせる。

Bot scopes:

- `chat:write`
- `app_mentions:read`
- `channels:history`
- `groups:history`

Bot events:

- `app_mention`
- `message.channels`
- `message.groups`

## マニフェストからアプリを作る

Slack の App Manifest API は呼ばない。UI で作る。

1. [Your Apps](https://api.slack.com/apps) → **Create New App** → **From an app manifest**
2. インストール先ワークスペースを選ぶ（withwork は XTalent / `TMGCWC4TY`。sae / tsumugi / hinata は Cajon / `T9U503RME`）
3. `apps/<slug>/manifest.yml` を貼る。`apps/akari/manifest.legacy.yml` は貼らない
4. 作成後、**Event Subscriptions → Request URL** を設定する。値は `https://<bypass-worker>` + 下表の path。`<bypass-worker>` は bypass Worker をデプロイしたあとの実ホスト。このリポジトリに hostname は書かない
5. **Install to Workspace**（権限の再承認が必要なら再インストール）
6. Basic Information の **Signing Secret** を `~/.slack-support-bypass/secrets.env` に入れる
7. 同じ `secrets.env` に Grok webhook URL / key を入れる（キー名は各 `apps/<slug>/README.md`）
8. `~/.slack-support-bypass/routes.json` にルート行を足す（shape は bypass リポジトリの `routes.example.json`）
9. 対象チャンネルに bot を invite する

| slug | App ID | path |
| --- | --- | --- |
| withwork | `A0C2DNVFPK8` | `/slack/withwork` |
| sae | `A0BUXNTB43U` | `/slack/hairbook` |
| tsumugi | `A0BUXEUUPM0` | `/slack/salonjobs` |
| hinata | `A0BUXV96DM0` | `/slack/synsalon` |

Bot Token (`xoxb-...`) は Grok Bot 側にだけ置く。このリポジトリにも bypass リポジトリにも置かない。Signing Secret もコミットしない。

### Request URL は後から

`event_subscriptions.request_url` はマニフェストに入れていない。公式 schema では任意だが、Events API を有効にするには Request URL か Socket Mode のどちらかが必要。受信は Events API の Request URL。bypass Worker の hostname をここに発明しない。Worker デプロイ後に Slack App 設定で入れる。

withwork の `manifest.yml` はイベント購読ブロック自体を置いていない。sae / tsumugi / hinata は `bot_events` だけ書き、`request_url` は空のまま。

Create from manifest の時点で Slack UI が Request URL を要求した場合は、いったん空のまま先に進められるなら進め、無理なら Worker デプロイ後に同じマニフェストを App Manifest エディタへ再適用する。

### `features.bot_user.display_name`

公式の [App manifest reference](https://docs.slack.dev/reference/app-manifest) は `bot_user.display_name` の許可文字を `a-z`, `0-9`, `-`, `_`, `.` と書いている（reference を 2026-10-07 に確認）。`display_information.name` は日本語でよい。withwork の bot 名は `marunoshin`、表示名は マル之進。App Settings の画面がドキュメントより広く日本語の bot 名を許すかは、Slack API を呼んでいないので未確認。

## Cajon のサポートアプリ

Hairbook / サロンジョブズ / シンサロンページは Cajon,Inc.（team `T9U503RME`）にインストールしたまま、マニフェストの正本をこのリポジトリに置く。

| アプリ | App ID | slug | bypass path |
| --- | --- | --- | --- |
| さえ｜Hairbookサポート | `A0BUXNTB43U` | `sae` | `POST /slack/hairbook` |
| 森崎 紬｜サロンジョブズ | `A0BUXEUUPM0` | `tsumugi` | `POST /slack/salonjobs` |
| 日向 ひなた｜シンサロンページ | `A0BUXV96DM0` | `hinata` | `POST /slack/synsalon` |

月島灯（`apps/akari`、App `A0BJYNERNG6`）の Slack アプリは 2026-10-07（JST）に削除済み。受信の後継は さえ。Socket Mode の記録は残し、推奨パスにはしない。同日に消した重複と Demo App、残しているプロダクトアプリは [APPS.md](./APPS.md)。

## License

[MIT](./LICENSE)（`aileron-inc/slack-support-bypass` と同じ）
