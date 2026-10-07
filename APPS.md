# Slack アプリ台帳

aileron-inc/slack-apps は Slack App のマニフェストと手順の置き場。Events Worker は [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass)。返信ロジックと Bot Token は Grok Bot 側。

App ID の正は、オーナーが貼った Slack「Your Apps」一覧（2026-10-07）。ワークスペース名もその一覧の表記（Cajon,Inc. / XTalent / Aileron inc）。team ID `T9U503RME` と `TMGCWC4TY` は、このリポジトリに以前から書いてあった値。Your Apps の貼り付けには team ID が無い。Aileron inc の team ID は未記載。

この一覧に対して Slack API は呼んでいない。Event URL の実ホストは確認していない。Request URL は `https://<bypass-worker>` + path で、hostname は書かない。

| 分類 | 意味 |
| --- | --- |
| keep-active | 稼働中の Event bot。マニフェストをこのリポジトリに置く |
| retired | 後継が決まっている廃止アプリ。記録だけ残す |
| retire-candidate | 重複または後継済み。削除候補。この PR では Slack から消していない |
| dormant | 残っているが、いまの受信パスとしては使っていない |
| keep-elsewhere | Cajon / XTalent のプロダクトアプリ、または記録のみ。作り直さない |
| personal/demo | 個人またはデモ |

## keep-active（マニフェストはここ）

| アプリ名 | App ID | slug | path | ワークスペース |
| --- | --- | --- | --- | --- |
| [さえ｜Hairbookサポート](apps/sae/README.md) | `A0BUXNTB43U` | sae | `/slack/hairbook` | Cajon,Inc. |
| [森崎 紬｜サロンジョブズ](apps/tsumugi/README.md) | `A0BUXEUUPM0` | tsumugi | `/slack/salonjobs` | Cajon,Inc. |
| [日向 ひなた｜シンサロンページ](apps/hinata/README.md) | `A0BUXV96DM0` | hinata | `/slack/synsalon` | Cajon,Inc. |
| [マル之進 (マルノシン)](apps/withwork/README.md) | `A0C2DNVFPK8` | withwork | `/slack/withwork` | XTalent |

さえの bot user は 2026-09-17 の `auth.test` で `hairbook_support`。マル之進の bot `display_name` は `marunoshin`。日向のライブ @mention は Your Apps に無い。紬の manifest は bypass 最小より広いスコープを export から残している。

## retired

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| [月島 灯｜Hairbookサポート](apps/akari/README.md) | `A0BJYNERNG6` | Cajon,Inc. | Socket Mode。後継は さえ `A0BUXNTB43U`。`manifest.legacy.yml` は記録。貼らない |

廃止手順は `apps/akari/README.md`。この PR ではアンインストールしていない。

## retire-candidate（重複・後継済み）

削除は未実施。同名アプリの Event URL は未確認。

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| 森崎 紬｜サロンジョブズ (local) | `A0BUR8478TV` | Cajon,Inc. | 本番 `A0BUXEUUPM0` の local 重複 |
| 森崎 紬｜サロンジョブズ | `A0BUGFXQNDD` | Cajon,Inc. | 本番と同じ表示名の別アプリ。bypass 本番は `A0BUXEUUPM0`。こちらがライブ Event URL だと証明されていない |
| アシスタント Mia (local) | `A0BV0BC2U2C` | Cajon,Inc. | Mia `A0BUQDC87BN` の local 重複 |
| Hairbook Support | `A0BUVGCE47Q` | Cajon,Inc. | 英語名の旧サポートアプリとみる。後継は さえ `A0BUXNTB43U` |
| withworkコロ助 | `A0C20A95WR5` | XTalent | 旧表示名。ライブの Event bot は マル之進 `A0C2DNVFPK8` |
| withwork-bot | `A041U5XSVQQ` | XTalent | より古い withwork bot。後継は マル之進 `A0C2DNVFPK8` |

## dormant

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| cmd | `A08QKUREZK2` | Cajon,Inc. | アーカイブ済みの hairbook-slack-cmd とみる。受信パスとしては使っていない |

## keep-elsewhere（記録のみ。作り直さない）

マニフェストは置かない。コードも移さない。

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| Raptor | `A0BQ58HJDLM` | Cajon,Inc. | `cajon-inc/raptor-bot` |
| Mia | `A0BUQDC87BN` | Cajon,Inc. | プロダクトの Mia。local 重複は `A0BV0BC2U2C` |
| cajon-bot | `A06LCPZTYLX` | Cajon,Inc. | noroshi-flash などを含むプロダクト bot |
| ノロシ | `A0C0N6Q96U8` | Cajon,Inc. | noroshi |
| sms-support | `A0C5DJ4NCJZ` | Cajon,Inc. | プロダクト / サポート |
| ai | `A0A6PBHPW8N` | XTalent | XTalent の ai |
| lumi | `A0BQZKEGQGL` | Cajon,Inc. | 記録のみ。置き場のリポジトリ名は Your Apps のメモに無い |
| 質問ちゃん | `A0C2ZPZG2P8` | Cajon,Inc. | Slack App がある（Grok MCP だけではない）。Events→Grok にするならマニフェストは別判断。この PR では置かない |
| カク之進 (カクノシン) | `A0C3LF4FNGY` | Cajon,Inc. | Slack App がある。MCP teammate がこの bot を使う可能性はオーナーメモ。この PR ではマニフェストを置かない |

## personal/demo

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| Demo App | `A0C2ANFQYUF` | Cajon,Inc. | デモ。削除するか残すかは未決。この PR では消さない |
| masa-dot | `A0C6GJ0LYUT` | Aileron inc | 個人アプリ |

## この Your Apps 一覧に無い名前

以前の相談メモにだけある。App ID は割り当てない。

| 名前 | メモ |
| --- | --- |
| magazine-review-bot | Your Apps に無い |
| mikin-daily-bot | Your Apps に無い |
| lead-status-bot | Your Apps に無い |
| incident-patrol | Your Apps に無い |
| withwork-slack-webhook | Your Apps にこの名前は無い。XTalent 側の withwork 系は上表の 3 アプリ |
| 柚希あかり（Grok） | Your Apps に無い。カク之進と質問ちゃんは Slack App として上表にある |
| `cajon-inc/hairbook-runbook-bot` | Your Apps の行には無い。日向のローカルメモが返信実装として指していた。Slack アプリの行は 日向 `A0BUXV96DM0` |

## ローカルクローンの重複

| パス | 何か | 正本 |
| --- | --- | --- |
| `~/Projects/slack-apps` | git 管理ではないローカルハブ（旧 README、アイコン、secrets へのリンク） | このリポジトリ（aileron-inc/slack-apps） |
| `~/Projects/slack-support-bypass` | 削除済み `cajon-inc` リモートを向いた orphan クローン | 下の行 |
| `~/Projects/slack-support-bypass-aileron` | Events Worker のクローン | [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass) |

この cloud agent からは `~/Projects` を見ていない。上の 3 行は相談結果としての記録。ハブの `secrets` や `routes.json`、Bot Token は git に入れない。
