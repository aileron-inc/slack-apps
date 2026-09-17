# slack-apps

Matchy の Slack App 管理リポジトリ。**マニフェストと作成・インストール手順だけ**を置く。

これは次のものではない。

- Events 転送 Worker（[aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass)）
- Grok Bot 側の返信ロジック

ランタイムもデーモンもここには置かない。シークレットも git に入れない。

## レイアウト

```
apps/<slug>/manifest.yml   # Slack app manifest (schema v2)
apps/<slug>/README.md      # そのアプリのインストール手順
```

いま管理しているアプリは `apps/withwork`（コロ助 / XTalent）だけ。

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
2. インストール先ワークスペースを選ぶ（withwork なら XTalent / `TMGCWC4TY`）
3. `apps/<slug>/manifest.yml` を貼る
4. 作成後、**Event Subscriptions → Request URL** を設定する。値は `https://<bypass-worker>` + ルート path（例: `/slack/withwork`）。`<bypass-worker>` は bypass Worker をデプロイしたあとの実ホスト。このリポジトリに hostname は書かない
5. **Install to Workspace**（権限の再承認が必要なら再インストール）
6. Basic Information の **Signing Secret** を `~/.slack-support-bypass/secrets.env` に入れる
7. 同じ `secrets.env` に Grok webhook URL / key を入れる（キー名は各 `apps/<slug>/README.md`）
8. `~/.slack-support-bypass/routes.json` にルート行を足す（shape は bypass リポジトリの `routes.example.json`）
9. 対象チャンネルに bot を invite する

Bot Token (`xoxb-...`) は Grok Bot 側にだけ置く。このリポジトリにも bypass リポジトリにも置かない。Signing Secret もコミットしない。

### Request URL は後から

`event_subscriptions.request_url` はマニフェストに入れていない。公式 schema では任意だが、Events API を有効にするには Request URL か Socket Mode のどちらかが必要。Socket Mode は使わない。bypass Worker の hostname をここに発明しない。Worker デプロイ後に Slack App 設定で入れる。

Create from manifest の時点で Slack UI が Request URL を要求した場合は、いったん空のまま先に進められるなら進め、無理なら Worker デプロイ後に同じマニフェストを App Manifest エディタへ再適用する。

### `features.bot_user.display_name`

公式の [App manifest reference](https://docs.slack.dev/reference/app-manifest) は `bot_user.display_name` の許可文字を `a-z`, `0-9`, `-`, `_`, `.` と書いている。日本語表示名（コロ助）は UI では普通に使われる。マニフェスト投入で弾かれたら `display_information.name` は日本語のまま、`bot_user.display_name` だけ `kolosuke` に落とす。未確認: App Settings のバリデーションがドキュメントどおり ASCII 限定かどうか。

## Cajon 既存アプリ（ここではまだ管理しない）

次は Cajon Slack (`T9U503RME`) で稼働中。このリポジトリでは再作成しない。移行するまで Cajon 側が正。

| アプリ | bypass path |
| --- | --- |
| Hairbookサポート | `POST /slack/hairbook` |
| サロンジョブズサポート | `POST /slack/salonjobs` |
| シンサロンページサポート | `POST /slack/synsalon` |

## License

[MIT](./LICENSE)（`aileron-inc/slack-support-bypass` と同じ）
