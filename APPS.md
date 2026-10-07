# Slack アプリ台帳

aileron-inc/slack-apps は Slack App のマニフェストと手順の置き場。Events Worker は [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass)。返信ロジックと Bot Token は Grok Bot 側。

ステータスは次のいずれか。

| ステータス | 意味 |
| --- | --- |
| `active` | 稼働中。マニフェストをこのリポジトリで管理する |
| `dormant` | 残っているが、いまの受信パスとしては使っていない |
| `retired` | 後継に置き換えた。マニフェストは記録だけ |
| `not-a-slack-app` | Slack App のマニフェスト置き場ではない。コードは移さない |
| `keep-on-cajon` | Cajon 側の実装が正本。このリポジトリへコードを移さない |

資料の範囲で `dormant` にしたものは無い。

## 出典

確認できたものと、この環境から見えないものを分ける。

- このリポジトリの `apps/withwork` と、ルート README に書いてあったワークスペース ID（XTalent `TMGCWC4TY`、Cajon `T9U503RME`）と bypass path。
- ローカルハブ `~/Projects/slack-apps` の README、Sae / Hinata のメモ、紬の manifest export、月島灯の Socket Mode export。ハブ README の日付は 2026-09-17。export の `workers.dev` ホストはマニフェストから外してある。
- 下の「Slack App リポジトリではない」と「ローカルの重複」は、オーナーの相談結果として記録する。この cloud agent からは `~/Projects` も private な cajon-inc リポジトリも見えていない。

## このリポジトリでマニフェストを管理する

インストール先のワークスペースはそのまま。マニフェストの正本はここ。

| slug | 表示名 | App ID | bot | ワークスペース | path | ステータス |
| --- | --- | --- | --- | --- | --- | --- |
| [withwork](apps/withwork/README.md) | マル之進 | `apps/withwork/README.md` には無い | `marunoshin` | XTalent `TMGCWC4TY` | `/slack/withwork` | active |
| [sae](apps/sae/README.md) | Sae｜Hairbookサポート | `A0BUXNTB43U` | `hairbook_support` | Cajon `T9U503RME` | `/slack/hairbook` | active |
| [tsumugi](apps/tsumugi/README.md) | 森崎 紬｜サロンジョブズ | `A0BUXEUUPM0` | `tsumugi` | Cajon `T9U503RME` | `/slack/salonjobs` | active |
| [hinata](apps/hinata/README.md) | 日向 | 未確認 | `hinata` | Cajon `T9U503RME` | `/slack/synsalon` | active |
| [akari](apps/akari/README.md) | 月島 灯｜Hairbookサポート | `A0BJYNERNG6` | `akari` | Cajon `T9U503RME` | 後継 sae の `/slack/hairbook` | retired |

Request URL は `https://<bypass-worker>` に path を足したもの。hostname はコミットしない。

Sae は月島灯の後継。Hinata の返信実装は下の `hairbook-runbook-bot` に残す。紬の manifest はインストール済み export のスコープを残している（bypass 最小より広い）。理由は各アプリの README。

## Cajon に実装を残す

| 名前 | ステータス | メモ |
| --- | --- | --- |
| `cajon-inc/hairbook-runbook-bot` | keep-on-cajon | Hinata のローカルメモが正本ボットとして指している。App ID はメモに無い。コードは移さない |

## Slack App リポジトリではない

コードは移さない。置き場所は現行の各リポジトリのまま（Cajon 側に残すもの、XTalent の webhook、Cursor teammate）。

| 名前 | ステータス | メモ |
| --- | --- | --- |
| Raptor | not-a-slack-app | Slack App のマニフェスト置き場ではない |
| magazine-review-bot | not-a-slack-app | Slack App のマニフェスト置き場ではない |
| mikin-daily-bot | not-a-slack-app | Slack App のマニフェスト置き場ではない |
| cajon-bot / noroshi-flash | not-a-slack-app | Slack App のマニフェスト置き場ではない |
| lead-status-bot | not-a-slack-app | Slack App のマニフェスト置き場ではない |
| incident-patrol | not-a-slack-app | Slack App のマニフェスト置き場ではない |
| withwork-slack-webhook | not-a-slack-app | XTalent 側の webhook。Slack App マニフェストの置き場ではない |
| カク之進、質問ちゃん、柚希あかり（Grok） | not-a-slack-app | Cursor teammate。Slack App リポジトリではない |

## ローカルの重複

| パス | 何か | 正本 |
| --- | --- | --- |
| `~/Projects/slack-apps` | git 管理ではないローカルハブ（旧 README、アイコン、secrets へのリンク） | このリポジトリ（aileron-inc/slack-apps） |
| `~/Projects/slack-support-bypass` | 削除済み `cajon-inc` リモートを向いた orphan クローン | 下の行 |
| `~/Projects/slack-support-bypass-aileron` | Events Worker のクローン | [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass) |

ハブの `secrets` や `routes.json`、Bot Token は git に入れない。
